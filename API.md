## API guide by [@Kilvoctu](https://github.com/Kilvoctu)

- 首先，当然是使用`--api`命令行参数运行Web UI。
  - 例如在您的"webui-user.bat"文件中：`set COMMANDLINE_ARGS=--api`
- 这将启用API，您可以在http://127.0.0.1:7860/docs（或其他URL + /docs）上查看API文档。
我对其中两个基本API感兴趣，让我们只关注 `/sdapi/v1/txt2img`。

![image](https://user-images.githubusercontent.com/2993060/198171114-ed1c5edd-76ce-4c34-ad73-04e388423162.png)

- 当您展开该标签时，它会提供一个向API发送的示例负载。我经常以此作为参考。

![image](https://user-images.githubusercontent.com/2993060/198171454-5b826ded-5e73-4249-9c0c-a97b32c42569.png)

------

- 这是后端的工作原理。API基本上说明了可用的内容、需要的内容以及发送位置。现在转向前端，我将开始构建一个带有我想要的参数的负载。一个示例可能是：
```py
payload = {
    "prompt": "maltese puppy",
    "steps": 5
}
```
我可以在负载中放入任意数量的参数，对于我未设置的任何参数，API将使用默认值。

- 然后，我可以将其发送到API：
```py
response = requests.post(url=f'http://127.0.0.1:7860/sdapi/v1/txt2img', json=payload)
```
再次强调，此URL需要与Web UI的URL匹配。
如果我们执行此代码，Web UI将根据负载生成图像。这很好，但是然后呢？没有图像可以找到...

------

在后端完成操作后，API将响应发送回上面分配的变量`response`。响应包含三个条目："images"、"parameters"和"info"，我需要找到一种方法来从这些条目中获取信息。
- 首先，我添加了这一行 `r = response.json()`，以便更方便地处理响应。
- "images"是生成的图像，这是我主要想要的。没有链接或其他内容；它是一个巨大的随机字符字符串，显然我们需要对其进行解码。我是这样做的：
```py
for i in r['images']:
    image = Image.open(io.BytesIO(base64.b64decode(i.split(",",1)[0])))
```
- 有了这个，我们可以在`image`变量中处理图像，例如使用`image.save('output.png')`保存图像。
- "parameters"显示了发送到API的内容，这可能是有用的，但在这种情况下我想要的是"info"。我使用它将元数据插入图像中，这样我就可以将其放入Web UI PNG Info中。为此，我可以访问`/sdapi/v1/png-info` API。我需要将上面得到的图像传递给它。
```py
png_payload = {
        "image": "data:image/png;base64," + i
    }
    response2 = requests.post(url=f'http://127.0.0.1:7860/sdapi/v1/png-info', json=png_payload)
```
然后，我可以使用`response2.json().get("info")`获取信息。

------

下面是一个可能有效的示例代码：
```py
import json
import requests
import io
import base64
from PIL import Image, PngImagePlugin

url = "http://127.0.0.1:7860"

payload = {
    "prompt": "puppy dog",
    "steps": 5
}

response = requests.post(url=f'{url}/sdapi/v1/txt2img', json=payload)

r = response.json()

for i in r['images']:
    image = Image.open(io.BytesIO(base64.b64decode(i.split(",",1)[0])))

    png_payload = {
        "image": "data:image/png;base64," + i
    }
    response2 = requests.post(url=f'{url}/sdapi/v1/png-info', json=png_payload)

    pnginfo = PngImagePlugin.PngInfo()
    pnginfo.add_text("parameters", response2.json().get("info"))
    image.save('output.png', pnginfo=pnginfo)
```
- 导入所需的库
- 定义URL和要发送的负载
- 通过API将负载发送到指定的URL
- 在循环中获取"images"并对其进行解码
- 对于每个图像，将其发送到png info API并获取信息
- 定义一个插件来添加PNG信息，然后将我定义的PNG信息添加到其中
- 在最后，使用PNG信息保存图像

-----

关于`"override_settings"`的说明。
此端点的目的是为单个请求覆盖Web UI的设置，例如CLIP跳过。可以通过此参数传递的设置在URL的`/docs`中可见。

![image](https://user-images.githubusercontent.com/2993060/202877368-c31a6e9e-0d05-40ec-ade0-49ed2c4be22b.png)

您可以展开选项卡，API将提供一个列表。有几种方法可以将此值添加到负载中，但这是我使用的方法。我将使用"filter_nsfw"和"CLIP_stop_at_last_layers"来演示。

```py
payload = {
    "prompt": "cirno",
    "steps": 20
}

override_settings = {}
override_settings["filter_nsfw"] = true
override_settings["CLIP_stop_at_last_layers"] = 2

override_payload = {
                "override_settings": override_settings
            }
payload.update(override_payload)
```
- 首先，创建一个正常的负载。
- 然后，初始化一个字典（我将其命名为"override_settings"，但这可能不是最好的名称）。
- 然后，可以向该字典中添加任意数量的键值对。
- 创建一个只包含此参数的新负载。
- 更新原始负载以添加此新负载。

因此，在这种情况下，当发送负载时，应该得到一个在20步时具有CLIP跳过为2的"cirno"，并启用了NSFW过滤器。

对于某些设置或情况，您可能希望您的更改保持不变。为此，您可以将其发布到`/sdapi/v1/options` API端点。我们可以根据我们之前学到的知识轻松设置代码。下面是一个示例：
```py
url = "http://127.0.0.1:7860"

option_payload = {
    "sd_model_checkpoint": "Anything-V3.0-pruned.ckpt [2700c435]",
    "CLIP_stop_at_last_layers": 2
}

response = requests.post(url=f'{url}/sdapi/v1/options', json=option_payload)
```
在将此负载发送到API后，模型应该切换到我设置的模型，并将CLIP跳过设置为2。需要再次强调，这与"override_settings"不同，因为此更改将持久存在，而"override_settings"仅适用于单个请求。
请注意，如果更改了`sd_model_checkpoint`，则值应为Web UI中显示的检查点(checkpoint(s))名称。可以使用此API端点引用它（与"options" API引用方式相同）。

![image](https://user-images.githubusercontent.com/2993060/202928589-114aff91-2777-4269-9492-2eab015c5bca.png)

您应该使用"标题"（名称和哈希值）。

-----

此信息基于提交[47a44c7](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/47a44c7e421b98ca07e92dbf88769b04c9e28f86)。

要获得更完整的前端实现，您可以查看我的Discord机器人[here](https://github.com/Kilvoctu/aiyabot)作为示例。大部分操作都在stablecog.py中进行，并且有许多注释解释每段代码的作用。

------

此指南可以在[discussions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3734)页面找到。

此外，请查看此用于Web UI的Python API客户端库：https://github.com/mix1009/sdwebuiapi
其中包含使用自定义脚本/扩展的示例：[here](https://github.com/mix1009/sdwebuiapi/commit/fe269dc2d4f8a98e96c63c8a7d3b5f039625bc18)
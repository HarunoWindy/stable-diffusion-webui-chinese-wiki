- **该程序经测试可在 Python 3.10.6 上运行。不要使用其他版本。**
- 程序需要16GB的常规内存才能平稳运行。如果您只有8GB的内存，请考虑创建一个8GB的页面file/swap文件，或者使用[--lowram](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Command-Line-Arguments-and-Settings)选项（如果您的GPU VRAM超过内存）。
- 安装程序会创建一个Python虚拟环境，因此安装的模块不会影响现有系统安装的Python。
- 要使用系统的Python而不是创建一个虚拟环境，请使用自定义参数替换`set VENV_DIR=-`。
- 要从头重新安装，请删除以下目录：`venv`、`repositories`。
- 当首次启动程序时，会显示Python解释器的路径。如果这不是您安装的Python解释器，您可以在`webui-user`脚本中指定完整路径；请参阅[Running with custom parameters](Run-with-Custom-Parameters)。
- 如果所需的Python版本不在PATH中，请在`webui-user.bat`文件中的`set PYTHON=python`行中修改为Python可执行文件的完整路径。
    - 示例：`set PYTHON=B:\soft\Python310\python.exe`
- 安装程序的要求来自`requirements_versions.txt`文件，该文件列出了与Python 3.10.6特别兼容的模块版本。如果这与其他版本的Python不兼容，设置自定义参数`set REQS_FILE=requirements.txt`可能会有所帮助。

# Low VRAM Video-cards
在使用VRAM较低（<=4GB）的显卡时，可能会出现内存不足的错误。
可以通过命令行参数启用各种优化，以减少显存使用，但会牺牲一些/很多速度：
- 如果VRAM = 4GB，并且想要生成 >= 512x512的图像，请使用`--medvram`。
- 如果VRAM = 4GB，并且想要生成 >= 512x512的图像，但使用`--medvram`时出现内存不足的错误，请改用`--lowvram --always-batch-cond-uncond`。
- 如果VRAM = 4GB，并且想要生成比`--medvram`支持的更大图像，请使用`--lowvram`。

# Torch is not able to use GPU
这是一个经常提到的问题，但通常不是WebUI的错误，有许多原因可能导致此问题。
- WebUI默认使用GPU，所以如果您没有适当的硬件，需要添加`--use-cpu`。
- 确保正确配置WebUI，请参考[wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki)中对应的安装教程。
- 如果在某些组件更新后遇到此问题，请尝试撤销最近的操作。

如果您是上述情况之一，应删除`venv`文件夹。

如果仍然无法解决问题，请在报告时提交一些附加信息。
1. 在`venv\Scripts`下打开控制台。
2. 运行`python -m torch.utils.collect_env`。
3. 将控制台的所有输出复制并发布。

# Green or Black screen
显卡
某些GPU显卡不支持半精度：生成的图片可能会显示为绿屏或黑屏。请使用`--upcast-sampling`。如果您正在使用`--xformers`，这个选项应该与之兼容。
如果问题仍未解决，可以使用命令行参数`--precision full --no-half`，但这会显著增加显存的使用，可能需要使用`--medvram`。

# "CUDA error: no kernel image is available for execution on the device" after enabling xformers
您安装的xformers与您的GPU不兼容。如果您使用的是Python 3.10，拥有Pascal或更高版本的显卡，并在Windows上运行，请在`COMMANDLINE_ARGS`中添加`--reinstall-xformers --xformers`以升级到可用版本。升级后，请删除`--reinstall-xformers`。

# NameError: name 'xformers' is not defined
如果你使用 Windows，这意味着你的 Python 太旧了。 使用 3.10

如果是 Linux，则必须自己构建 xformers 或避免使用 xformers。

# `--share` non-functional after gradio 3.22 update

Windows Defender/防病毒软件有时会阻止Gradio创建公共URL的能力。

1. 打开您的防病毒软件。
2. 检查保护历史：
   ![image](https://user-images.githubusercontent.com/98228077/229028161-4ad3c837-ae3f-45f7-9a0a-fa165d70d943.png)
3. 将其添加到排除列表中。

相关问题：
<details>

https://github.com/gradio-app/gradio/issues/3230 \
https://github.com/gradio-app/gradio/issues/3677
</details>

# weird css loading

![image](https://user-images.githubusercontent.com/98228077/229085355-0fbd56d6-fe1c-4858-8701-6c5697b9a6d6.png)

This issue has been noted, 3 times. It is apparently something users in china may experience.
这个问题被提到过3次。 这是中国用户可能会遇到的事情
[#8537](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/8537)

<details><summary> Solution: </summary>

这个问题是因为我的电脑注册表中的CSS文件类型信息有误，导致CSS解析和应用出错。
解决方案：
![image](https://user-images.githubusercontent.com/98228077/229086022-f27858a3-c9d9-470c-87cc-aa1974b7c5d0.png)


根据上图定位，修改最后的Content Type和PerceivedType。
最后重启机器，删除浏览器缓存，强制刷新网页 (shift+f5).
Thanks to https://www.bilibili.com/read/cv19519519
</details>
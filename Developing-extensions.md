[Adding to Index](#Official-Extension-Index)
扩展只是`extensions`目录中的一个子目录。

Web UI与已安装扩展的交互方式如下：

- 如果存在扩展的`install.py`脚本，则执行该脚本。
- `scripts`目录中的扩展脚本会被执行，就像它们只是普通的用户脚本一样，除了以下几点：
  - `sys.path`会被扩展以包含扩展目录，这样您就可以无需担心导入扩展目录中的任何内容。
  - 您可以使用`scripts.basedir()`来获取当前扩展目录（因为用户可以将其命名为任何他想要的名称）。
- `javascript`目录中的扩展JavaScript文件会被添加到页面中。
- `localizations`目录中的扩展本地化文件会被添加到设置中；如果有两个同名的本地化文件，它们不会合并，而是一个替换另一个。
- 扩展的`style.css`文件会被添加到页面中。
- 如果扩展的根目录中有`preload.py`文件，它会在解析命令行参数之前加载。
- 如果扩展的`preload.py`中有一个`preload`函数，它会被调用，并将命令行参数解析器作为参数传递给它。以下是如何使用它来添加命令行参数的示例：

```python
def preload(parser):
    parser.add_argument("--wildcards-dir", type=str, help="directory with wildcards", default=None)
```

对于如何开发通常会完成大部分扩展工作的自定义脚本， 请参考 [Developing custom scripts](Developing-custom-scripts).

## Localization extensions
项目中首选的本地化方法是通过创建一个扩展来实现。扩展的基本文件结构应该是：

```

 📁 webui root directory
 ┗━━ 📁 extensions
     ┗━━ 📁 webui-localization-la_LA        <----- name of extension
         ┗━━ 📁 localizations                <----- the single directory inside the extension
             ┗━━ 📄 la_LA.json              <----- actual file with translations
```

请创建一个具有以下文件结构的 GitHub 存储库，并请协作者部分列出的任何人将您的扩展添加到 Wiki 中。

如果您的语言需要 JavaScript/CSS 或甚至 Python 支持，您也可以将其添加到扩展中。

## install.py
`install.py`是由`launch.py`启动的脚本，启动器，在WebUI启动之前以单独的进程运行，并且其目的是安装扩展的依赖项。它必须位于扩展的根目录中，而不是脚本目录中。脚本会使用设置为WebUI路径的`PYTHONPATH`环境变量启动，因此您只需`import launch`并使用其功能即可：

```python
import launch

if not launch.is_installed("aitextgen"):
    launch.run_pip("install aitextgen==0.6.0", "requirements for MagicPrompt")
```

## Minor tips
### Adding extra textual inversion dirs
This code goes into extension's script:
```python
path = os.path.join(modules.scripts.basedir(), "embeddings")
modules.sd_hijack.model_hijack.embedding_db.add_embedding_dir(path)
```
## User Examples
https://github.com/udon-universe/stable-diffusion-webui-extension-templates \
https://github.com/AliceQAQ/sd-webui-gradio-demo \
https://github.com/wcdnail/sd-web-ui-wexperimental

## Official Extension Index
- Add extensions here - https://github.com/AUTOMATIC1111/stable-diffusion-webui-extensions

(此外，您可以在此处添加 extensions+webui 的工作提交版本: )

- https://github.com/camenduru/sd-webui-extension-records

## Internals Diagram by [@hananbeer](https://github.com/hananbeer)
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/8601

https://miro.com/app/board/uXjVMdgY-TY=/?share_link_id=547908852229

![image](https://user-images.githubusercontent.com/98228077/229259967-15556a72-774c-44ba-bab5-687f854a0fc7.png)


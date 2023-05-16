# Required Dependencies
1. Python 3.10.6和Git：
 - Windows：下载并运行Python 3.10.6([网页](https://www.python.org/downloads/release/python-3106/)，[exe](https://www.python.org/ftp/python/3.10.6/python-3.10.6-amd64.exe)或[win7版本](https://github.com/adang1345/PythonWin7/raw/master/3.10.6/python-3.10.6-amd64-full.exe))和git([网页](https://git-scm.com/download/win))的安装程序。
 - Linux (基于Debian)：`sudo apt install wget git python3 python3-venv`
 - Linux (基于Red Hat)：`sudo dnf install wget git python3`
 - Linux (基于Arch)：`sudo pacman -S wget git python3`
2. 来自此存储库的代码：
 - 首选方式：使用git：`git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git`。
 - 这种方式被推荐是因为它允许您通过运行`git pull`来更新。
 - 这些命令可以从在资源管理器中右键单击并选择“Git Bash here”后打开的命令行窗口中使用。
 - 另一种方式：在存储库的主页上使用“Code”(绿色按钮)->“Download ZIP”选项。
 - 即使您选择这种方式，您仍然需要安装git。
 - 要更新，您必须再次下载zip并替换文件。

# Optional Dependencies

## ESRGAN (Upscaling)
额外的微调ESRGAN模型，如[模型数据库](https://upscale.wiki/wiki/Model_Database)中的模型，可以放置到ESRGAN目录中。在您第一次运行之前，ESRGAN目录并不存在于存储库中。

如果模型具有`.pth`扩展名，它将作为一个模型加载，并在用户界面(UI)中以其名称显示。

> 注意：RealESRGAN模型不是ESRGAN模型，它们不兼容。不要下载RealESRGAN模型。不要将RealESRGAN放置到带有ESRGAN模型的目录中。

# Automatic Installation
## Windows (method 1)

> 一个非常基础的指南，用于在Windows 10/11 NVIDIA GPU上启动并运行Stable Diffusion web UI。1. 从[v1.0.0-pre](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.0.0-pre)下载`sd.webui.zip`并解压缩该zip文件。2. 双击`update.bat`脚本以将web UI更新到最新版本，等待完成后关闭窗口。3. 双击`run.bat`脚本以启动web UI，在首次启动时它将下载大量文件。在所有内容都已正确下载和安装后，您应该看到一条消息“`Running on local URL: http://127.0.0.1:7860`”，打开链接将呈现给您web UI界面。
> 您应该能够开始生成图像

### Extra configurations via `COMMANDLINE_ARGS`
您可能希望将一些配置选项应用于web UI，为了配置这些选项，您需要编辑位于`sd.webui\webui\webui-user.bat`的启动脚本，在文件中编辑，在`set COMMANDLINE_ARGS=`后添加所选参数，如下所示：
```bat
set COMMANDLINE_ARGS=--autolaunch --update-check
```
> 每个单独的参数都需要用空格分隔，上面的示例将配置web UI在完成加载后自动启动浏览器页面，并在启动时检查web UI的新版本。

### Troubleshooting
Web UI的默认配置应该能在大多数GPU上运行，但在某些情况下，您可能需要一些额外的参数才能使其正常工作。

1. 对于VRAM较少的GPU，您可能需要`--medvram`或`--lowvram`，这些优化减少了VRAM的需求，但牺牲了性能。 如果您没有足够的VRAM，web UI可能拒绝启动或由于内存不足而无法生成图像(image)。 所需的VRAM量很大程度上取决于所需的图像分辨率，有关更多详细信息，请参见[Troubleshooting](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Troubleshooting)。
> [Tiled VAE](https://github.com/pkuliyi2015/multidiffusion-upscaler-for-automatic1111)扩展可以帮助减少VRAM的需求。

2. 如果您生成的结果是黑色或绿色图像(image)，请尝试添加`--precision full`和`--no-half`。

3. 某些模型(model)和VAE的组合容易产生`NansException: A tensor with all NaNs was produced in VAE`，从而导致黑色图像(image)，使用选项`--no-half-vae`可能有助于缓解此问题。

### Extra Options
1. 有几种交叉衰减优化方法，例如`--xformers`或`--opt-sdp-attention`，这些方法可以大大提高性能，有关更多详细信息，请参见[Optimizations](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Optimizations)，并尝试不同的选项，因为不同的硬件适用于不同的优化。 如果您希望测量系统的性能，请尝试使用[sd-extension-system-info](https://github.com/vladmandic/sd-extension-system-info)扩展，它具有基准测试工具和用户提交结果的[数据库](https://vladmandic.github.io/sd-extension-system-info/pages/benchmark.html)。
2. 添加`--autolaunch`，在web UI启动后自动启动web浏览器。
3. 添加`--update-check`将在webui有新版本时通知您。
4. 有关更多配置选项，请参见[Command Line Arguments and Settings
](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Command-Line-Arguments-and-Settings)。

### Tip
如果您已经下载了Stable Diffusion模型，您可以在运行第3步中的`run.bat`之前，将模型移动到`sd.webui\webui\models\Stable-diffusion\`，这将跳过自动下载[stable-diffusion-v1-5 model](https://huggingface.co/runwayml/stable-diffusion-v1-5)模型。

## Windows (method 2)
1. 安装[Python 3.10.6](https://www.python.org/ftp/python/3.10.6/python-3.10.6-amd64.exe)（勾选**Add to PATH**），和[git](https://github.com/git-for-windows/git/releases/download/v2.39.2.windows.1/Git-2.39.2-64-bit.exe)
2. 从搜索栏打开命令提示符，并输入`git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui`
3. 双击`webui-user.bat`

如果您遇到困难，可以参考安装视频：\
<sup>解决[#8229](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/8229)</sup>

<details><summary>视频：（点击展开）：</summary>

https://user-images.githubusercontent.com/98228077/223032534-c5dd5b13-a4b6-47a7-995c-27ed8ba8b3e7.mp4

</details>

<details><summary>替代的Powershell启动脚本：</summary>

**webui.ps1**

```
if ($env:PYTHON -eq "" -or $env:PYTHON -eq $null) {
    $PYTHON = "Python.exe"
} else {
    $PYTHON = $env:PYTHON
}

if ($env:VENV_DIR -eq "" -or $env:VENV_DIR -eq $null) {
    $VENV_DIR = "$PSScriptRoot\venv"
} else {
    $VENV_DIR = $env:VENV_DIR
}

if ($env:LAUNCH_SCRIPT -eq "" -or $env:LAUNCH_SCRIPT -eq $null) {
    $LAUNCH_SCRIPT = "$PSScriptRoot\launch.py"
} else {
    $LAUNCH_SCRIPT = $env:LAUNCH_SCRIPT
}

$ERROR_REPORTING = $false

mkdir tmp 2>$null

function Start-Venv {
    if ($VENV_DIR -eq '-') {
        Skip-Venv
    }

    if (Test-Path -Path "$VENV_DIR\Scripts\$python") {
        Activate-Venv
    } else {
        $PYTHON_FULLNAME = & $PYTHON -c "import sys; print(sys.executable)"
        Write-Output "Creating venv in directory $VENV_DIR using python $PYTHON_FULLNAME"
        Invoke-Expression "$PYTHON_FULLNAME -m venv $VENV_DIR > tmp/stdout.txt 2> tmp/stderr.txt"
        if ($LASTEXITCODE -eq 0) {
            Activate-Venv
        } else {
            Write-Output "Unable to create venv in directory $VENV_DIR"
        }
    }
}

function Activate-Venv {
    $PYTHON = "$VENV_DIR\Scripts\Python.exe"
    $ACTIVATE = "$VENV_DIR\Scripts\activate.bat"
    Invoke-Expression "cmd.exe /c $ACTIVATE"
    Write-Output "Venv set to $VENV_DIR."
    if ($ACCELERATE -eq 'True') {
        Check-Accelerate
    } else {
        Launch-App
    }
}

function Skip-Venv {
    Write-Output "Venv set to $VENV_DIR."
    if ($ACCELERATE -eq 'True') {
        Check-Accelerate
    } else {
        Launch-App
    }
}

function Check-Accelerate {
    Write-Output 'Checking for accelerate'
    $ACCELERATE = "$VENV_DIR\Scripts\accelerate.exe"
    if (Test-Path -Path $ACCELERATE) {
        Accelerate-Launch
    } else {
        Launch-App
    }
}

function Launch-App {
    Write-Output "Launching with python"
    Invoke-Expression "$PYTHON $LAUNCH_SCRIPT"
    #pause
    exit
}

function Accelerate-Launch {
    Write-Output 'Accelerating'
    Invoke-Expression "$ACCELERATE launch --num_cpu_threads_per_process=6 $LAUNCH_SCRIPT"
    #pause
    exit
}


try {
    if(Get-Command $PYTHON){
        Start-Venv
    }
} Catch {
    Write-Output "Couldn't launch python."
}
```


**webui-user.ps1**

```
[Environment]::SetEnvironmentVariable("PYTHON", "")
[Environment]::SetEnvironmentVariable("GIT", "")
[Environment]::SetEnvironmentVariable("VENV_DIR","")

# Commandline arguments for webui.py, for example: [Environment]::SetEnvironmentVariable("COMMANDLINE_ARGS", "--medvram --opt-split-attention")
[Environment]::SetEnvironmentVariable("COMMANDLINE_ARGS", "")

# script to launch to start the app
# [Environment]::SetEnvironmentVariable("LAUNCH_SCRIPT", "launch.py")

# install command for torch
# [Environment]::SetEnvironmentVariable("TORCH_COMMAND", "pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 --extra-index-url https://download.pytorch.org/whl/cu113")

# Requirements file to use for stable-diffusion-webui
# [Environment]::SetEnvironmentVariable("REQS_FILE", "requirements_versions.txt")

# [Environment]::SetEnvironmentVariable("GFPGAN_PACKAGE", "")
# [Environment]::SetEnvironmentVariable("CLIP_PACKAGE", "")
# [Environment]::SetEnvironmentVariable("OPENCLIP_PACKAGE", "")

# URL to a WHL if you wish to override default xformers windows
# [Environment]::SetEnvironmentVariable("XFORMERS_WINDOWS_PACKAGE", "")

# Uncomment and set to enable an alternate repository URL
# [Environment]::SetEnvironmentVariable("STABLE_DIFFUSION_REPO", "")
# [Environment]::SetEnvironmentVariable("TAMING_TRANSFORMERS_REPO", "")
# [Environment]::SetEnvironmentVariable("K_DIFFUSION_REPO", "")
# [Environment]::SetEnvironmentVariable("CODEFORMER_REPO", "")
# [Environment]::SetEnvironmentVariable("BLIP_REPO", "")

# Uncomment and set to enable a specific revision of a repository
# [Environment]::SetEnvironmentVariable("STABLE_DIFFUSION_COMMIT_HASH", "")
# [Environment]::SetEnvironmentVariable("TAMING_TRANSFORMERS_COMMIT_HASH", "")
# [Environment]::SetEnvironmentVariable("K_DIFFUSION_COMMIT_HASH", "")
# [Environment]::SetEnvironmentVariable("CODEFORMER_COMMIT_HASH", "")
# [Environment]::SetEnvironmentVariable("BLIP_COMMIT_HASH", "")


# Uncomment to enable accelerated launch
# [Environment]::SetEnvironmentVariable("ACCELERATE", "True")

$SCRIPT = "$PSScriptRoot\webui.ps1"
Invoke-Expression "$SCRIPT"
```

</details>




如果出现问题，请参阅[Troubleshooting](Troubleshooting)部分。



## Third party installation guides/scripts:
- NixOS: https://github.com/virchau13/automatic1111-webui-nix

# Almost Automatic Installation and Launch
要通过pip安装所需的软件包而不创建虚拟环境，请运行：
```bash
python launch.py
```

可以直接传递命令行参数，例如：
```bash
python launch.py --opt-split-attention --ckpt ../secret/anime9999.ckpt
```

# Manual Installation
手动安装非常过时，可能无法正常工作。请在仓库的自述文件中查看colab以获取说明。

以下过程在Windows或Linux上手动安装所有内容（后者需要将`dir`替换为`ls`）：
```bash
# 安装支持CUDA的torch。如果失败，请参阅https://pytorch.org/get-started/locally/获取更多说明。
pip install torch --extra-index-url https://download.pytorch.org/whl/cu113

# 检查torch是否支持GPU；这必须输出“True”。您需要安装CUDA 11.。您可能可以使用
# 不同的版本，但这是我测试过的。
python -c "import torch; print(torch.cuda.is_available())"

# clone web ui and go into its directory
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
cd stable-diffusion-webui

# 克隆Stable Diffusion和（可选）CodeFormer的仓库
mkdir repositories
git clone https://github.com/CompVis/stable-diffusion.git repositories/stable-diffusion
git clone https://github.com/CompVis/taming-transformers.git repositories/taming-transformers
git clone https://github.com/sczhou/CodeFormer.git repositories/CodeFormer
git clone https://github.com/salesforce/BLIP.git repositories/BLIP

# install requirements of Stable Diffusion
pip install transformers==4.19.2 diffusers invisible-watermark --prefer-binary

# install k-diffusion
pip install git+https://github.com/crowsonkb/k-diffusion.git --prefer-binary

# (optional) install GFPGAN (face restoration)
pip install git+https://github.com/TencentARC/GFPGAN.git --prefer-binary

# (optional) install requirements for CodeFormer (face restoration)
pip install -r repositories/CodeFormer/requirements.txt --prefer-binary

# install requirements of web ui
pip install -r requirements.txt  --prefer-binary

# update numpy to latest version
pip install -U numpy  --prefer-binary

# （在命令行外）将Stable Diffusion模型放入web ui目录中
# 下面的命令必须输出类似于：1 File(s) 4,265,380,512 bytes的内容
dir model.ckpt

```

The installation is finished, to start the web ui, run:
```bash
python webui.py
```

# Windows 11 WSL2 instructions
To install under a Linux distro in Windows 11's WSL2:
```bash
# install conda (if not already done)
wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
chmod +x Anaconda3-2022.05-Linux-x86_64.sh
./Anaconda3-2022.05-Linux-x86_64.sh

# Clone webui repo
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
cd stable-diffusion-webui

# Create and activate conda env
conda env create -f environment-wsl2.yaml
conda activate automatic

```
此时，可以从步骤`# clone repositories for Stable Diffusion and (optionally) CodeFormer`开始应用手动安装的说明。

# Alternative installation on Windows using Conda
- Prerequisites _*(Only needed if you do not have them)*_. Assumes [Chocolatey](https://chocolatey.org/install) is installed. 
    ```bash
    # install git
    choco install git
    # install conda
    choco install anaconda3
    ```
    Optional parameters: [git](https://community.chocolatey.org/packages/git), [conda](https://community.chocolatey.org/packages/anaconda3)
- 安装（警告：某些文件超过多个千兆字节，请确保您首先有足够的空间）
  1. Download as .zip and extract or use git to clone.
        ```bash
        git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
        ```
  2. 启动Anaconda提示符。应该注意的是，您可以使用较旧的Python版本，但您可能会被迫手动删除诸如缓存优化之类的功能，这将降低您的性能。
        ```bash
        # Navigate to the git directory
        cd "GIT\StableDiffusion"
        # Create environment
        conda create -n StableDiffusion python=3.10.6
        # Activate environment
        conda activate StableDiffusion
        # Validate environment is selected
        conda env list
        # Start local webserver
        webui-user.bat
        # Wait for "Running on local URL:  http://127.0.0.1:7860" and open that URI.
        ```
    3. _*(Optional)*_ Go to [CompVis](https://huggingface.co/CompVis) and download latest model, for example [1.4](https://huggingface.co/CompVis/stable-diffusion-v1-4) and unpack it to ex:
        ```bash
        GIT\StableDiffusion\models\Stable-diffusion
        ```
        after that restart the server by restarting Anaconda prompt and 
        ```bash
        webui-user.bat
        ```
- 值得尝试的替代默认值：
1. 尝试**euler a**（Ancestral Euler），并将**Sampling Steps**提高到40或其他100。
2. 将“设置>用户界面>每N个采样步骤显示图像创建进度”设置为1，并选择确定性的**Seed**值。可以直观地看到图像去扩散是如何发生的，并使用[ScreenToGif](https://github.com/NickeManarin/ScreenToGif)记录a.gif。
3. 使用**Restore faces**。通常，效果更好，但这种质量是以速度为代价的。


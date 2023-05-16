Xformers库是一种可选的加速图像生成的方法。

这种优化仅适用于nvidia显卡，它可以加速图像生成并降低显存使用量，但代价是产生[non-deterministic](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2705#discussioncomment-4024378)的结果。

注意：Windows和linux都不再需要构建。在2023年1月23日，webui从用户构建的轮子转向[official wheel](https://pypi.org/project/xformers/0.0.16rc425/#history)，并进行了其他软件包升级，如[this PR](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/5939/commits/c091cf1b4acd2047644d3571bcbfd81c81b4c3af)所示。

如果您使用Pascal、Turing、Ampere、Lovelace或Hopper显卡，并且使用Python 3.10，则可以使用`--xformers`启动repo，并安装兼容的轮子。

## Building xformers on Windows by [@duckness](https://github.com/duckness)

1. [Install VS Build Tools 2022](https://visualstudio.microsoft.com/downloads/?q=build+tools#build-tools-for-visual-studio-2022), you only need `Desktop development with C++`

![setup_COFbK0AJAZ](https://user-images.githubusercontent.com/6380270/194767872-232136a1-9204-4b16-ae21-3e01f6f526ea.png)

2. [Install CUDA 11.3](https://developer.nvidia.com/cuda-11.3.0-download-archive) (later versions are not tested), select custom, you only need the following (VS integration is probably unecessary):

![setup_QwCdsQ28FM](https://user-images.githubusercontent.com/6380270/194767963-6df7ce14-e6eb-4718-8e93-a11abf172f14.png)

3. Clone the [xFormers repo](https://github.com/facebookresearch/xformers), create a `venv` and activate it

```sh
git clone https://github.com/facebookresearch/xformers.git
cd xformers
git submodule update --init --recursive
python -m venv venv
./venv/scripts/activate
```

4. To avoid issues with getting the CPU version, [install pyTorch seperately](https://pytorch.org/get-started/locally/):

```sh
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu113
```

5. Then install the rest of the dependencies:

```sh
pip install -r requirements.txt
pip install wheel
```

由于CUDA 11.3相当旧，您需要强制启用它在MS Build Tools 2022上构建。如果在`powershell`上，请执行`$env:NVCC_FLAGS = "-allow-unsupported-compiler"`；如果在`cmd`上，请执行`set NVCC_FLAGS=-allow-unsupported-compiler`。


您最终可以构建xFormers，但请注意，构建将需要很长时间（可能需要10-20分钟），它最初可能会抱怨一些错误，但仍应正确编译。

> 可选提示：要进一步加速多核CPU Windows系统，请安装ninja https://github.com/ninja-build/ninja。
> 安装步骤：
> 1. 从https://github.com/ninja-build/ninja/releases下载ninja-win.zip并解压缩
> 2. 将ninja.exe放在C:\Windows下或将提取的ninja.exe的完整路径添加到系统PATH中
> 3. 在cmd中运行ninja -h并验证是否看到打印了帮助消息
> 4. 运行以下命令开始构建。它应该自动使用Ninja，不需要额外的配置。您应该看到显著更高的CPU使用率（40%+）。
> ```
> python setup.py build
> python setup.py bdist_wheel
> ```
> 这将AMD 5800X CPU的Windows PC的构建时间从1.5小时缩短到10分钟。
> Ninja也支持Linux和MacOS，但我没有这些操作系统进行测试，因此无法提供逐步教程。



8. Run the following:
 ```sh
python setup.py build
python setup.py bdist_wheel
```

9. In `xformers` directory, navigate to the `dist` folder and copy the `.whl` file to the base directory of `stable-diffusion-webui`

10. In `stable-diffusion-webui` directory, install the `.whl`, change the name of the file in the command below if the name is different:

```sh
./venv/scripts/activate
pip install xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl
```

11. Ensure that `xformers` is activated by launching `stable-diffusion-webui` with `--force-enable-xformers`

## Building xformers on Linux (from anonymous user)

1. go to the webui directory
2. `source ./venv/bin/activate`
3. `cd repositories`
3. `git clone https://github.com/facebookresearch/xformers.git`
4. `cd xformers`
5. `git submodule update --init --recursive`
6. `pip install -r requirements.txt`
7. `pip install -e .`

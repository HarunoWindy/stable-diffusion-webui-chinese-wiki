## Environment variables

| name                   | description                                                                                                                               |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| PYTHON                 | sets a custom path for Python executable                                                                                                  |
| VENV_DIR               | specifies the path for the virtual environment. Default is `venv`. Special value `-` runs the script without creating virtual environment |
| COMMANDLINE_ARGS       | additional commandline arguments for the main program                                                                                     |
| IGNORE_CMD_ARGS_ERRORS | set to anything to make the program not exit with an error if an unedxpected commandline argument is encountered                          |
| REQS_FILE              | name of requirements.txt file with dependencies that wuill be installed when `launch.py` is run. Defaults to `requirements_versions.txt`  |
| TORCH_COMMAND          | command for installing pytorch                                                                                                            |
| INDEX_URL              | --index-url parameter for pip                                                                                                             |
| TRANSFORMERS_CACHE     | path to where transformers library will download and keep its files related to the CLIP model                                             |
| CUDA_VISIBLE_DEVICES   | select gpu to use for your instance on a system with multiple gpus. For example if you want to use secondary gpu, put "1".<br>(add a new line to webui-user.bat not in COMMANDLINE_ARGS): `set CUDA_VISIBLE_DEVICES=0`<br>Alternatively, just use `--device-id` flag in COMMANDLINE_ARGS. |

### webui-user
建议通过编辑`webui-user.bat`（Windows）和`webui-user.sh`（Linux）来指定环境变量：
- `set VARNAME=VALUE` for Windows
- `export VARNAME="VALUE"` for Linux

For example, in Windows:
```
set COMMANDLINE_ARGS=--allow-code --xformers --skip-torch-cuda-test --no-half-vae --api --ckpt-dir A:\\stable-diffusion-checkpoints 
```

## Command Line Arguments
### Running online
使用`--share`选项在线运行。您将获得一个xxx.app.gradio链接。这是在协作中使用该程序的预期方式。您可以使用`--gradio-auth username:password`标志为gradio共享实例设置身份验证，可选地提供用逗号分隔的多组用户名和密码。

使用`--listen`使服务器监听网络连接。这将允许本地网络上的计算机访问UI，如果您配置了端口转发，也可以让互联网上的计算机访问。

使用`--port xxxx`使服务器监听特定端口，xxxx为所需端口。请记住，所有低于1024的端口都需要root/admin权限，因此建议使用高于1024的端口。如果可用，默认为端口7860。

# All command line arguments

| Argument Command | Value | Default | Description |
| ---------------- | ----- | ------- | ----------- |
| **CONFIGURATION** |
-h, --help | None | False | 显示此帮助信息并退出 |
--exit |  |  | 安装后终止 |
--data-dir | DATA_DIR | ./ | 存储所有用户数据的基本路径 |
--config | CONFIG | configs/stable-diffusion/v1-inference.yaml | 构建模型的配置路径 |
--ckpt | CKPT | model.ckpt | 稳定扩散模型的检查点路径；如果指定，此检查点将被添加到检查点列表并加载 |
--ckpt-dir | CKPT_DIR | None | 稳定扩散检查点目录的路径 |
--no-download-sd-model | None | False | 即使没有找到模型也不下载SD1.5模型 |
--vae-dir | VAE_PATH | None | 变分自编码器模型的路径；禁用与VAE相关的所有设置
--vae-path | VAE_PATH | None | 用作VAE的检查点；设置此参数
--gfpgan-dir| GFPGAN_DIR | GFPGAN/ | GFPGAN目录 |
--gfpgan-model| GFPGAN_MODEL | GFPGAN模型文件名 |
--codeformer-models-path | CODEFORMER_MODELS_PATH | models/Codeformer/ | codeformer模型文件目录的路径。 |
--gfpgan-models-path ｜ GFPGAN_MODELS_PATH ｜ models/GFPGAN ｜GFPGAN模型文件目录的路径。|
--esrgan-models-path ｜ ESRGAN_MODELS_PATH ｜ models/ESRGAN ｜ ESRGAN模型文件目录的路径。|
--bsrgan-models-path ｜ BSRGAN_MODELS_PATH ｜ models/BSRGAN ｜ BSRGAN模型文件目录的路径。|
--realesrgan-models-path ｜ REALESRGAN_MODELS_PATH ｜ models/RealESRGAN ｜ RealESRGAN模型文件目录的路径。|
--scunet-models-path ｜ SCUNET_MODELS_PATH ｜ models/ScuNET ｜ ScuNET模型文件目录的路径。|
--swinir-models-path ｜ SWINIR_MODELS_PATH ｜ models/SwinIR ｜ SwinIR和SwinIR v2模型文件目录的路径。|
--ldsr-models-path ｜ LDSR_MODELS_PATH ｜ models/LDSR ｜ LDSR模型文件目录的路径。|
--lora-dir ｜ LORA_DIR ｜ models/Lora ｜ Lora网络目录的路径。|
--clip-models-path ｜ CLIP_MODELS_PATH ｜ None ｜ CLIP模型文件目录的路径。|
--embeddings-dir ｜ EMBEDDINGS_DIR ｜ embeddings/ ｜ 文本反演嵌入目录（默认：embeddings）|
--textual-inversion-templates-dir ｜ TEXTUAL_INVERSION_TEMPLATES_DIR ｜ textual_inversion_templates ｜ 文本反演模板目录
--hypernetwork-dir ｜ HYPERNETWORK_DIR ｜ models/hypernetworks/ ｜ 超网络目录|
--localizations-dir ｜ LOCALIZATIONS_DIR ｜ localizations/ ｜ localizations 目录|
--styles-file ｜ STYLES_FILE ｜ styles.csv ｜ 样式使用的文件名|
--ui-config-file ｜ UI_CONFIG_FILE ｜ ui-config.json | 用户界面配置使用的文件名 |
--no-progressbar-hiding | None | False | 不隐藏gradio UI中的进度条（我们隐藏它是因为如果您在浏览器中有硬件加速，它会减慢ML） |
--max-batch-count| MAX_BATCH_COUNT | 16 | UI的最大批次计数值 |
--ui-settings-file | UI_SETTINGS_FILE | config.json | 用户界面设置使用的文件名 |
--allow-code | None | False | 允许从webui执行自定义脚本 |
--share | None | False | 使用share=True for gradio并使UI通过他们的站点访问（对我不起作用，但您可能更幸运）
--listen | None | False | 使用0.0.0.0作为服务器名称启动gradio，允许响应网络请求 |
--port | PORT | 7860 | 使用给定的服务器端口启动gradio，对于端口<1024，您需要root/admin权限，默认为7860（如果可用）|
--hide-ui-dir-config | None | False | 隐藏webui中的目录配置 |
--freeze-settings | None | False | 禁用编辑设置 |
--enable-insecure-extension-access | None | False | 不管其他选项如何，都启用扩展选项卡 |
--gradio-debug | None | False | 使用--debug选项启动gradio |
--gradio-auth | GRADIO_AUTH | None | 设置gradio身份验证，如"username:password"；或逗号分隔多个，如"u1:p1,u2:p2,u3:p3" |
--gradio-auth-path | GRADIO_AUTH_PATH | None | 设置gradio身份验证文件路径，例如"/path/to/auth/file"，与`--gradio-auth`相同的身份验证格式 |
--disable-console-progressbars | None | False | 不要将进度条输出到控制台 |
--enable-console-prompts | None | False | 在使用txt2img和img2img生成时将提示打印到控制台 |
--api | None | False | 使用API启动webui |
--api-auth | API_AUTH | None | 设置API身份验证，如"username:password"；或逗号分隔多个，如"u1:p1,u2:p2,u3:p3" |
--api-log | None | False | 启用所有API请求的日志记录 |
--nowebui | None | False | 仅启动API，不启动UI |
--ui-debug-mode | None | False | 不加载模型以快速启动UI |
--device-id | DEVICE_ID | None | 选择要使用的默认CUDA设备（可能需要在之前导出CUDA_VISIBLE_DEVICES=0,1等）|
--administrator | None | False | 管理员权限 |
--cors-allow-origins ｜ CORS_ALLOW_ORIGINS ｜ None ｜ 允许CORS源的形式为逗号分隔列表（无空格）|
--cors-allow-origins-regex ｜ CORS_ALLOW_ORIGINS_REGEX ｜ None ｜ 允许CORS源的形式为单个正则表达式|
--tls-keyfile ｜ TLS_KEYFILE ｜ None ｜ 部分启用TLS，需要--tls-certfile才能完全运行|
--tls-certfile ｜ TLS_CERTFILE ｜ None ｜ 部分启用TLS，需要--tls-keyfile才能完全运行|
--disable-tls-verify | None | False | 传递时，启用自签名证书(self-signed certificates)的使用。|
--server-name | SERVER_NAME | None | 设置服务器主机名 |
--no-gradio-queue | None| False | 禁用gradio队列；使网页使用http请求而不是websockets；在早期版本中是默认值
--no-hashing | None | False | 禁用检查点的sha256哈希以帮助提高加载性能 |
--skip-version-check | None | False | 不检查torch和xformers的版本 |
--skip-python-version-check | None | False | 不检查Python版本 |
--skip-torch-cuda-test | None | False | 不检查CUDA是否能正常工作 |
--skip-install | None | False | 跳过安装软件包 |
| **PERFORMANCE** |
--xformers | None | False | 为交叉注意层启用xformers |
--force-enable-xformers | None | False | 无论检查代码是否认为您可以运行它，都启用交叉注意层的xformers；***如果无法正常工作，请不要提交错误报告*** |
--xformers-flash-attention | None | False | 启用带有Flash Attention的xformers以提高可重复性（仅支持SD2.x或变体）|
--opt-sdp-attention | None | False | 启用缩放点积交叉注意层优化；需要PyTorch 2.*|
--opt-sdp-no-mem-attention | False | None | 启用不带内存高效注意力的缩放点积交叉注意层优化，使图像生成确定性；需要PyTorch 2.*|
--opt-split-attention | None | False | 强制启用Doggettx的交叉注意层优化。默认情况下，它对cuda启用的系统有效。|
--opt-split-attention-invokeai | None | False | 强制启用InvokeAI的交叉注意层优化。默认情况下，当cuda不可用时启用。|
--opt-split-attention-v1 | None | False | 启用旧版本的拆分注意力优化，不会消耗它能找到的所有VRAM |
--opt-sub-quad-attention ｜ None ｜ False ｜ 启用内存高效子二次交叉注意层优化|
--sub-quad-q-chunk-size ｜ SUB_QUAD_Q_CHUNK_SIZE ｜ 1024 ｜ 子二次交叉注意层优化使用的查询块大小|
--sub-quad-kv-chunk-size ｜ SUB_QUAD_KV_CHUNK_SIZE ｜ None ｜ 子二次交叉注意层优化使用的kv块大小|
--sub-quad-chunk-threshold ｜ SUB_QUAD_CHUNK_THRESHOLD ｜ None ｜ 子二次交叉注意层优化使用分块的VRAM百分比阈值|
--opt-channelslast ｜ None ｜ False ｜ 启用4d张量的替代布局，可能会导致更快的推理**only**在具有张量核心（16xx及更高版本）的Nvidia卡上|
--disable-opt-split-attention ｜ None ｜ False ｜ 强制禁用交叉注意层优化|
--disable-nan-check | None | False | 不检查生成的图像/潜在空间是否有nans；在CI中无检查点运行时有用|
--use-cpu | {all, sd, interrogate, gfpgan, bsrgan, esrgan, scunet, codeformer} | None | 为指定模块使用CPU作为torch设备 |
--no-half | None | False | 不将模型切换为16位浮点数 |
--precision | {full,autocast} | autocast | 以此精度评估 |
--no-half-vae | None | False | 不将VAE模型切换为16位浮点数 |
--upcast-sampling ｜ None ｜ False ｜ 上采样。对--no-half无效。通常产生与--no-half相似的结果，性能更好，同时使用更少的内存。|
--medvram ｜ None ｜ False ｜ 启用稳定扩散模型优化，以牺牲一点速度来获得低VRM使用率|
--lowvram ｜ None ｜ False ｜ 启用稳定扩散模型优化，以牺牲大量速度来获得非常低的VRM使用率|
--lowram ｜ None ｜ False ｜ 将稳定扩散检查点权重加载到VRAM而不是RAM中|
--always-batch-cond-uncond ｜ None ｜ False ｜ 禁用通过--medvram或--lowvram启用的cond/uncond批处理，以节省内存|
| **FEATURES** |
--autolaunch | None | False | 启动时在系统默认浏览器中打开webui URL |
--theme | None | Unset | 以指定主题（“light”或“dark”）打开webui。如果未指定，则使用默认浏览器主题 |
--use-textbox-seed | None | False | 在UI中使用文本框进行种子（no up/down, but possible to input long seeds）|
--disable-safe-unpickle ｜ None ｜ False ｜ 禁用检查pytorch模型中的恶意代码|
--ngrok ｜ NGROK ｜ None ｜ ngrok authtoken，gradio --share的替代方案|
--ngrok-region ｜ NGROK_REGION ｜ us ｜ ngrok应启动的区域。|
--update-check ｜ None ｜ None ｜ 启动时，通知您的webui版本（commit）是否与che当前主分支保持最新。|
--update-all-extensions ｜ None ｜ None ｜ 启动时，为您安装的所有扩展程序拉取最新更新。|
--reinstall-xformers ｜ None ｜ False ｜ 强制重新安装xformers。升级时有用-但升级后删除它，否则您将永远重新安装xformers。|
--reinstall-torch ｜ None ｜ False ｜ 强制重新安装torch。升级时有用-但升级后删除它，否则您将永远重新安装torch。|
--tests ｜ TESTS ｜ False ｜ 运行测试以验证webui功能，请参阅wiki主题了解更多详细信息。|
--no-tests ｜ None ｜ False ｜ 即使指定了--tests选项也不运行测试|
| **DEFUNCT OPTIONS** |
--show-negative-prompt | None | False | 没有任何作用 |
--deepdanbooru | None | False | 没有任何作用 |
--unload-gfpgan | None | False | 没有任何作用。|
--gradio-img2img-tool ｜ GRADIO_IMG2IMG_TOOL ｜ None ｜ 没有任何作用|
--gradio-inpaint-tool ｜ GRADIO_INPAINT_TOOL ｜ None ｜ 没有任何作用|
--gradio-queue ｜ None ｜ False ｜ 没有任何作用|

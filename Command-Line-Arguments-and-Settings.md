## Environment variables

| name                   | description                                                                                                                               |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| PYTHON                 | 为 Python设置自定义路径                                                                                                                    |
| VENV_DIR               | 指定虚拟环境的路径。 默认为`venv`。 使用特殊值`-`运行脚本而不创建虚拟环境                                                                      |
| COMMANDLINE_ARGS       | 主程序的附加命令行参数                                                                                                                      |
| IGNORE_CMD_ARGS_ERRORS | 设置为任何值以使程序在遇到意外的命令行参数错误而退出                                                                                           |
| REQS_FILE              | `requirements.txt`文件的名称，其中包含在运行`launch.py`时将安装的依赖项。 默认为`requirements_versions.txt`                                    |
| TORCH_COMMAND          | command for installing pytorch                                                                                                            |
| INDEX_URL              | --index-url parameter for pip                                                                                                             |
| TRANSFORMERS_CACHE     | transformers 库与CLIP模型相关的文件保存路径                                   |
| CUDA_VISIBLE_DEVICES   | 多个GPU的系统上选择要用使用的GPU。 例如，如果您想使用第2个GPU，请输入`1`。<br>(在 webui-user.bat中添加一个新行，而不是在COMMANDLINE_ARGS中): `set CUDA_VISIBLE_DEVICES=0`<br或者，just use `--device-id` flag in COMMANDLINE_ARGS. |

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
使用`--share`选项在线运行。您将获得一个xxx.app.gradio链接。这是在colabs中使用该程序的方式。您可以使用`--gradio-auth username:password`flag为构建共享实例设置身份验证，可选地填写用户名和密码，多组用逗号分隔。

使用`--listen`使服务器监听网络连接。这将允许本地网络上的计算机访问UI，如果您配置了端口转发，也可以让互联网上的计算机访问。

使用`--port xxxx`使服务器监听特定端口，xxxx为所需端口，默认7860(所有<1024的端口都需要root/admin权限，因此建议使用>1024的端口)。

# All command line arguments

| Argument Command | Value | Default | Description |
| ---------------- | ----- | ------- | ----------- |
| **CONFIGURATION** |
-h, --help | None | False | show this help message and exit |
--exit |  |  | 安装后终止 |
--data-dir | DATA_DIR | ./ | 存储所有用户数据的基本路径 |
--config | CONFIG | configs/stable-diffusion/v1-inference.yaml | 构建模型的配置路径 |
--ckpt | CKPT | model.ckpt | stable diffusion model的checkpoint路径；指定checkpoint文件可以将该checkpoint添加到checkpoints list并加载 |
--ckpt-dir | CKPT_DIR | None | stable diffusion checkpoints目录的路径 |
--no-download-sd-model | None | False | 即使没有找到模型也不下载SD1.5模型 |
--vae-dir | VAE_PATH | None | VAE模型的路径  |
--vae-path | VAE_PATH | None | 用作VAE的checkpoint时设置此参数 |
--gfpgan-dir| GFPGAN_DIR | GFPGAN/ | GFPGAN目录 |
--gfpgan-model| GFPGAN_MODEL | GFPGAN模型文件名 |
--codeformer-models-path | CODEFORMER_MODELS_PATH | models/Codeformer/ | codeformer模型文件目录的路径。|
--gfpgan-models-path | GFPGAN_MODELS_PATH | models/GFPGAN | GFPGAN模型文件目录的路径 |
--esrgan-models-path | ESRGAN_MODELS_PATH | models/ESRGAN | ESRGAN模型文件目录的路径 |
--bsrgan-models-path | BSRGAN_MODELS_PATH | models/BSRGAN | BSRGAN模型文件目录的路径 |
--realesrgan-models-path | REALESRGAN_MODELS_PATH | models/RealESRGAN | RealESRGAN模型文件目录的路径 |
--scunet-models-path | SCUNET_MODELS_PATH | models/ScuNET | ScuNET模型文件目录的路径 |
--swinir-models-path | SWINIR_MODELS_PATH | models/SwinIR | SwinIR和SwinIR v2模型文件目录的路径 |
--ldsr-models-path | LDSR_MODELS_PATH | models/LDSR	| LDSR模型文件目录的路径 |
--lora-dir | LORA_DIR | models/Lora | Lora网络目录的路径
--clip-models-path | CLIP_MODELS_PATH | None | CLIP模型文件目录的路径 |
--embeddings-dir | EMBEDDINGS_DIR | embeddings/		 | textual inversion的embeddings目录 |
--textual-inversion-templates-dir | TEXTUAL_INVERSION_TEMPLATES_DIR | textual_inversion_templates | textual inversion模板目录
--hypernetwork-dir | HYPERNETWORK_DIR | models/hypernetworks/	 | hypernetwork directory |
--localizations-dir | LOCALIZATIONS_DIR | localizations/ | localizations directory
--styles-file | STYLES_FILE | styles.csv 				| styles filename |
--ui-config-file | UI_CONFIG_FILE | 	ui-config.json	| ui configuration filename |
--no-progressbar-hiding | None | False | 不隐藏构建UI中的进度条（如果浏览器中有硬件加速，设为显示会减慢ML） |
--max-batch-count| MAX_BATCH_COUNT | 16 | UI的最大批次计数值 |
--ui-settings-file | UI_SETTINGS_FILE | config.json | UI设置使用的文件名 |
--allow-code | None | False | 允许从webui执行自定义脚本 |
--share | None | False | 使用share=True构建并使UI通过他们的站点访问（对我不起作用，但您可能更幸运）
--listen | None | False | 使用0.0.0.0作为服务器名称启动构建，允许响应网络请求 |
--port | PORT | 7860 | 使用给定的服务器端口启动构建，对于端口<1024，您需要root/admin权限，默认为7860（如果可用）|
--hide-ui-dir-config | None | False | 隐藏webui中的目录配置 |
--freeze-settings | None | False | 禁用编辑设置 |
--enable-insecure-extension-access | None | False | 强制启用启用扩展tab(不管其他选项) |
--gradio-debug | None | False | 使用--debug选项启动构建 |
--gradio-auth | GRADIO_AUTH | None | 设置构建身份验证，如"username:password"；或逗号分隔多个，如"u1:p1,u2:p2,u3:p3" |
--gradio-auth-path | GRADIO_AUTH_PATH | None | 设置构建身份验证文件路径，例如"/path/to/auth/file"，与`--gradio-auth`相同的身份验证格式 |
--disable-console-progressbars | None | False | 不要将进度条输出到控制台 |
--enable-console-prompts | None | False | 在使用txt2img和img2img生成时将提示打印到控制台 |
--api | None | False | 使用API启动webui |
--api-auth | API_AUTH | None | 设置API身份验证，如"username:password"；或逗号分隔多个，如"u1:p1,u2:p2,u3:p3" |
--api-log | None | False | 启用所有API请求的日志记录 |
--nowebui | None | False | 仅启动API，不启动UI |
--ui-debug-mode | None | False | 不加载模型以快速启动UI |
--device-id | DEVICE_ID | None | 选择要使用的默认CUDA设备（可能需要在之前导出CUDA_VISIBLE_DEVICES=0,1等）|
--administrator | None | False | 管理员权限 |
--cors-allow-origins | CORS_ALLOW_ORIGINS | None | 允许的CORS origin(s)，逗号分隔（无空格） |
--cors-allow-origins-regex | CORS_ALLOW_ORIGINS_REGEX | None | 允许的CORS origin(s)为单个正则表达式 |
--tls-keyfile | TLS_KEYFILE | None | 部分启用TLS，完全运行需要--tls-certfile |
--tls-certfile | TLS_CERTFILE | None | 部分启用TLS，完全运行需要--tls-keyfile |
--disable-tls-verify | None | False | 传递时，启用自签名证书(self-signed certificates)的使用。|
--server-name | SERVER_NAME | None | 设置服务器主机名 |
--no-gradio-queue | None| False | 禁用构建队列；使网页使用http请求而不是websockets；在早期版本中是默认值
--no-hashing | None | False | 禁用检查点(checkpoint(s))的sha256哈希以帮助提高加载性能 |
--skip-version-check | None | False | 不检查torch和xformers的版本 |
--skip-python-version-check | None | False | 不检查Python版本 |
--skip-torch-cuda-test | None | False | 不检查CUDA是否能正常工作 |
--skip-install | None | False | 跳过安装软件包 |
| **PERFORMANCE** |
--xformers | None | False | 为cross attention layers启用xformers |
--force-enable-xformers | None | False | 强制为cross attention layers启用xformers；***如果无法正常工作，请不要提交错误报告*** |
--xformers-flash-attention | None | False | 为Flash Attention启用xformers以提高再生性（仅支持SD2.x系列版本）|
--opt-sdp-attention | None | False | 启用scaled dot product cross-attention layer优化；需要PyTorch 2.* |
--opt-sdp-no-mem-attention | False | None | 启用不带memory efficient attention的scaled dot product cross-attention layer优化，使图像生成更确定；需要PyTorch 2.*|
--opt-split-attention | None | False | 强制启用Doggettx's cross-attention layer优化。默认情况下，它对cuda启用的系统有效。|
--opt-split-attention-invokeai | None | False | 强制启用InvokeAI's cross-attention layer优化。默认情况下，当cuda不可用时启用。|
--opt-split-attention-v1 | None | False | 启用旧版本的split attention优化，不会消耗它能找到的所有VRAM |
--opt-sub-quad-attention | None | False | 启用内存加速sub-quadratic cross-attention layer优化
--sub-quad-q-chunk-size | SUB_QUAD_Q_CHUNK_SIZE | 1024 | sub-quadratic cross-attention layer优化使用的查询块大小
--sub-quad-kv-chunk-size | SUB_QUAD_KV_CHUNK_SIZE | None | sub-quadratic cross-attention layer优化使用的kv块大小
--sub-quad-chunk-threshold | SUB_QUAD_CHUNK_THRESHOLD | None | sub-quadratic cross-attention layer优化使用分块的VRAM百分比阈值
--opt-channelslast | None | False    					| 启用4d张量的替代布局，在具有Tensor cores (16xx+)的Nvidia卡上增加推理速度 |
--disable-opt-split-attention | None | False 			| 强制禁用cross-attention layer优化 |
--disable-nan-check | None | False | 不检查生成的图像/潜在空间(latent space)是否有nans；在CI中无checkpoint运行时有用|
--use-cpu | {all, sd, interrogate, gfpgan, bsrgan, esrgan, scunet, codeformer} | None | 为指定模块使用CPU作为torch设备 |
--no-half | None | False | 不将模型切换为16位浮点数 |
--precision | {full,autocast} | autocast | 以此精度评估 |
--no-half-vae | None | False | 不将VAE模型切换为16位浮点数 |
--upcast-sampling | None | False | upcast sampling。对--no-half无效。通常产生与--no-half相似的结果，但性能更好，使用更少的内存
--medvram    | None | False          				 | 启用stable diffusion模型优化，以牺牲速度换取低VRM使用率 |
--lowvram    | None | False          				 | 启用stable diffusion模型优化，以牺牲大量速度换取非常低的VRM使用率 |
--lowram     | None | False         				 | 将stable diffusion checkpoint权重加载到VRAM而不是RAM中
--always-batch-cond-uncond | None | False			 | 禁用通过--medvram或--lowvram启用的cond/uncond批处理，以节省内存
| **FEATURES** |
--autolaunch | None | False | 启动时在系统默认浏览器中打开webui URL |
--theme | None | Unset | 以指定主题（“light”或“dark”）打开webui。如果未指定，则使用默认浏览器主题 |
--use-textbox-seed | None | False | 在UI中使用种子文本框（no up/down, but possible to input long seeds）|
--disable-safe-unpickle | None | False				| 禁用检查pytorch模型中的恶意代码 |
--ngrok | NGROK | None         				 | ngrok authtoken, 替代gradio --share
--ngrok-region | NGROK_REGION | us			 | ngrok启动的区域
--update-check | None | None | 启动时，通知您的webui版本（commit）是否最新
--update-all-extensions | None | None | 启动时，为您安装的所有扩展程序拉取最新更新
--reinstall-xformers | None | False | 强制重新安装xformers。升级时有用-但升级后需要删除，否则每次都会重新安装xformers |
--reinstall-torch | None | False | 强制重新安装torch。升级时有用-但升级后需要删除，否则每次都会重新安装torch |
--tests | TESTS | False | 运行测试以验证webui功能，请参阅wiki主题了解更多详细信息
--no-tests | None | False | 即使指定了--tests选项也不运行测试
| **DEFUNCT OPTIONS** |
--show-negative-prompt | None | False 					| does not do anything |
--deepdanbooru | None | False 					| does not do anything |
--unload-gfpgan | None | False      				 | does not do anything.
--gradio-img2img-tool | GRADIO_IMG2IMG_TOOL | None | does not do anything |
--gradio-inpaint-tool | GRADIO_INPAINT_TOOL | None | does not do anything |
--gradio-queue | None | False | does not do anything |

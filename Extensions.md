# General Info

扩展是一种更方便使用的user scripts。

扩展都存在于webui的扩展文件夹中的自己的文件夹中。 您可以使用git安装扩展，如下所示：

 git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients

这将从https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients 安装扩展到extensions/aesthetic-gradients目录中。

或者，您可以直接复制粘贴一个目录到扩展中。

有关开发扩展，请参阅[Developing extensions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Extensions/Developing-extensions)。

# Security

由于扩展程序允许用户安装和运行任意代码，这可能会被恶意使用，因此在使用允许远程用户连接到服务器的选项(`--share`或`--listen`)运行时默认禁用 - 你仍然可以使用界面，但尝试安装任何东西都将导致错误。如果您想使用这些选项并仍能安装扩展程序，请使用`--enable-insecure-extension-access`命令行标志。

# Extensions

## MultiDiffusion with Tiled VAE
https://github.com/pkuliyi2015/multidiffusion-upscaler-for-automatic1111

### MultiDiffusion

- `txt2img` panorama 生成，如MultiDiffusion中所述。
- 它可以与ControlNet配合生成带有控制的宽幅图像。

Panorama Example:
Before: [click for the raw image](https://github.com/pkuliyi2015/multidiffusion-upscaler-for-automatic1111/blob/docs/imgs/ancient_city_origin.jpeg)
After: [click for the raw image](https://github.com/pkuliyi2015/multidiffusion-upscaler-for-automatic1111/blob/docs/imgs/ancient_city.jpeg)

ControlNet Canny Output: https://github.com/pkuliyi2015/multidiffusion-upscaler-for-automatic1111/raw/docs/imgs/yourname.jpeg?raw=true

### Tiled Vae

`vae_optimize.py`脚本是一个疯狂的黑客，它将图像分成多个tile，分别对每个tile进行编码，然后将结果合并在一起。 这个过程允许VAE在有限的VRAM（8K图像约10 GB！）上处理巨大的图像。

删除--lowvram和--medvram即可享受！

## VRAM Estimator
https://github.com/space-nuko/a1111-stable-diffusion-webui-vram-estimator

多维度，多batch sizes运行txt2img, img2img, highres-fix 直至内存溢出(OOM)，把过程数据记录到图表中.

![image](https://user-images.githubusercontent.com/98228077/223624383-545aeb31-c001-4ba6-bdb8-23e688130b8f.png)

## Dump U-Net
https://github.com/hnmr293/stable-diffusion-webui-dumpunet

View different layers, observe U-Net feature maps. 允许通过为 unet 的每个block提供不同的prompts来生成图像: https://note.com/kohya_ss/n/n93b7c01b0547

![image](https://user-images.githubusercontent.com/98228077/223624012-2df926d5-d4c4-44bc-a04f-bed15d43b88f.png)

## posex
https://github.com/hnmr293/posex

Pose2Image的预估图像生成器。此扩展允许在3D中移动OpenPose人物形象。

![image](https://user-images.githubusercontent.com/98228077/223622234-26907947-a723-4671-ae42-60a0011bfda2.png)

	
## LLuL
https://github.com/hnmr293/sd-webui-llul

Local Latent Upscaler. 指定一个区域以选择性地增强细节。

https://user-images.githubusercontent.com/120772120/221390831-9fbccdf8-5898-4515-b988-d6733e8af3f1.mp4


## CFG-Schedule-for-Automatic1111-SD
https://github.com/guzuligo/CFG-Schedule-for-Automatic1111-SD

这两个脚本允许在生成step的期间动态控制CFG。 通过正确的设置，即使在img2img中降噪强度较低(low denoising)，也可以帮助获得高CFG的细节，而不会损坏生成的图像(image)。

See their [wiki](https://github.com/guzuligo/CFG-Schedule-for-Automatic1111-SD/wiki/CFG-Auto-script) on how to use.

## a1111-sd-webui-locon
https://github.com/KohakuBlueleaf/a1111-sd-webui-locon
An extension for loading LoCon networks in webui.

## ebsynth_utility
https://github.com/s9roll7/ebsynth_utility

一个使用img2img和ebsynth创建视频的扩展。输出使用ebsynth编辑的视频。与ControlNet一起使用。

![image](https://user-images.githubusercontent.com/98228077/223622872-0575abe9-9a53-4614-b9a5-1333f0b34733.png)


## Lora Block Weight

LoRA是一个功能强大的工具，但有时使用起来可能有困难，而且可能影响您不想影响的区域。此脚本允许逐个block设置权重。使用此脚本，您可能能够获得所需的图像。

关联XY坐标图一起使用，可以检查层次结构的每个级别的影响。

![image](https://user-images.githubusercontent.com/98228077/223573538-d8fdb00d-6c49-47ec-af63-cea691f515d4.png)

Included Presets:

```
NOT:0,0,0,0,0,0,0,0,0,0,0,0,0,0,0 
ALL:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1 
INS:1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0
IND:1,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0,0,0
INALL:1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0
MIDD:1,0,0,0,1,1,1,1,1,1,1,1,1,0,0,0,0,0
OUTD:1,0,0,0,0,0,0,0,0,1,1,1,1,1,0,0,0,0
OUTS:1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1
OUTALL:1,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1
```

## Kitchen Theme
https://github.com/canisminor1990/sd-web-ui-kitchen-theme

A custom theme for webui.

![image](https://user-images.githubusercontent.com/98228077/223572989-e51eb877-74f4-41ec-8204-233221f2e981.png)


## Bilingual Localization
https://github.com/journey-ad/sd-webui-bilingual-localization

双语翻译，再也不用为找不到原文按钮而烦恼了。 兼容语言包扩展，无需重新导入。

![image](https://user-images.githubusercontent.com/98228077/223564624-61594e71-d1dd-4f32-9293-9697b07a7735.png)

## Composable Lora
https://github.com/opparco/stable-diffusion-webui-composable-lora

启用使用 AND 关键字（可组合扩散）将 LoRA 限制为子提示。 与 Latent Couple 扩展配对时很有用。

## Clip Interrogator
https://github.com/pharmapsychotic/clip-interrogator-ext

Clip Interrogator是一个由pharmapsychotic移植而来的拓展. 具有多种clip models 和 interrogate settings。

![image](https://user-images.githubusercontent.com/98228077/223572478-093030bf-e25e-42c0-b621-597515deaf69.png)

## Latent-Couple
https://github.com/opparco/stable-diffusion-webui-two-shot

Composable Diffusion的内置拓展, 用于确定subprompts 反映的 latent space的区域.

![image](https://user-images.githubusercontent.com/98228077/223571685-95a300f9-b768-4bca-96d4-684aadae9863.png)


## OpenPose Editor
https://github.com/fkunn1326/openpose-editor

这可以添加多个姿势角色，从图像中检测姿势，保存到 PNG，并发送到 controlnet 扩展。

![image](https://user-images.githubusercontent.com/98228077/223571127-6c107bd8-7ca4-4774-bdb8-41930863fdcc.png)


## SuperMerger
https://github.com/hako-mikan/sd-webui-supermerger

合并并运行而不保存到磁盘。Sequential XY 合并生成；提取和合并 loras，将 loras 绑定到 ckpt，merge block weights等等。

![image](https://user-images.githubusercontent.com/98228077/223570729-d25ca5c4-a434-42fd-b85d-7e16a7af1fc8.png)

## Prompt Translator
https://github.com/butaixianran/Stable-Diffusion-Webui-Prompt-Translator

使用Deepl或百度将prompt翻译成英文的集成翻译器。

![image](https://user-images.githubusercontent.com/98228077/223565541-43f618ea-e009-41b5-880c-7360a9ebec5f.png)

## Video Loopback
https://github.com/fishslot/video_loopback_for_webui

https://user-images.githubusercontent.com/122792358/218375476-a4116c74-5a9a-41e2-970a-c3cc09f796ae.mp4

## Mine Diffusion
https://github.com/fropych/mine-diffusion

此扩展程序将图像转换为blocks并创建原理图(schematics)，以便使用 Litematica mod 轻松导入到 Minecraft 中。

<details><summary>Example: (Click to expand:)</summary>

![](https://github.com/fropych/mine-diffusion/blob/master/README_images/demo.gif)

</details>

## anti-burn
https://github.com/klimaleksus/stable-diffusion-webui-anti-burn

跳过最后几个steps然后把之前steps生成的一些图像融合在一起以平滑生成的图像。

![image](https://user-images.githubusercontent.com/98228077/223562829-1abe8eed-dca5-4891-88e2-6714966e02bc.png)

## Embedding Merge
https://github.com/klimaleksus/stable-diffusion-webui-embedding-merge

Merging Textual Inversion embeddings at runtime from string literals.

![image](https://user-images.githubusercontent.com/98228077/223562706-af7cc1e6-7b6c-4069-a89f-8087a2dba4da.png)

## gif2gif

此脚本的目的是接受动画gif作为输入，像img2img一样处理帧，然后将它们重新组合成一个动画gif。旨在提供一个有趣、快速的gif到gif工作流程，支持新模型和方法，如Controlnet和InstructPix2Pix。只需放入gif文件即可开始。参考了来自prompts_from_file的代码。

<details><summary>Example: (Click to expand:)</summary>

![](https://user-images.githubusercontent.com/93007558/216803715-81dfc9e6-8c9a-47d5-9879-27acfac34eb8.gif)

</details>

## cafe-aesthetic
https://github.com/p1atdev/stable-diffusion-webui-cafe-aesthetic

Pre-trained model，判断图片艺术感和分类。 Also has a tab with Batch processing.

![image](https://user-images.githubusercontent.com/98228077/223562229-cba2db0a-3368-4f13-9456-ebe2053c01a3.png)


## Catppuccin themes
https://github.com/catppuccin/stable-diffusion-webui

Catppuccin 是一个社区驱动的柔和主题，旨在成为低对比度主题和高对比度主题之间的中间地带。 添加一组符合 catppucin 指南的主题。

![image](https://user-images.githubusercontent.com/98228077/223562461-13ec3132-4734-4787-a161-b2c408646835.png)


## Dynamic Thresholding
动态阈值添加可定制的动态阈值，以允许高CFG比例(scale)值而不会出现燃烧/“流行艺术”('pop art')效果。

添加可定制的动态阈值，以允许高CFG比例(scale)值而不会出现燃烧/“('pop art')流行艺术”效果。


## Custom Diffusion
https://github.com/guaneec/custom-diffusion-webui

简而言之，自定义扩散是在TI的基础上进行微调，而不是微调整个模型。与TI具有类似的速度和内存需求，据说可以在更少的步骤中获得更好的结果。

## Fusion
https://github.com/ljleb/prompt-fusion-extension

该工具在采样步骤期间添加了prompt-travel和shift-attention-like interpolations(see exts)。Always-on + works w/ existing prompt-editing syntax。多种插值模式。有关更多信息，请参见其wiki页面。

<details><summary>Example: (Click to expand:)</summary>

![](https://user-images.githubusercontent.com/32277961/214725976-b72bafc6-0c5d-4491-9c95-b73da41da082.gif)

</details>

## Pixelization
https://github.com/AUTOMATIC1111/stable-diffusion-webui-pixelization

Using pre-trained models, 生成像素风图片 in the extras tab.

![image](https://user-images.githubusercontent.com/98228077/223563687-cb0eb3fe-0fce-4822-8170-20b719f394fa.png)

			
## Instruct-pix2pix
https://github.com/Klace/stable-diffusion-webui-instruct-pix2pix

增加了一个tab，可以使用instruct-pix2pix model做img2img editing。The author added the feature to webui, so this doesn't need to be used.

## System Info
https://github.com/vladmandic/sd-extension-system-info

在自动WebUI中创建一个顶级的**系统信息(System Info)**选项卡，其中包含：

* 系统负载(System Load)信息，包括CPU、内存和磁盘使用情况；
* 网络信息(Network)，包括内部和外部IP地址、网关、DNS和网络带宽；
* Python环境信息(Python Environment)，包括安装路径、版本和已安装的包列表；
* 系统信息(System Info)，包括操作系统名称、版本、主机名和CPU架构；
* 状态(State)和内存(Memory)信息，如果选项卡可见，则每秒自动更新(选项卡不可见时不进行更新)。

注意：
- 如果选项卡可见，则状态和内存信息每秒自动更新(选项卡不可见时不进行更新)。
- 所有其他信息在WebUI加载时会被更新一次，并且可以在需要时强制刷新。

![screenshot](https://raw.githubusercontent.com/vladmandic/sd-extension-system-info/main/system-info.jpg)

## Steps Animation
https://github.com/vladmandic/sd-extension-steps-animation

该扩展可以从降噪(denoised)的中间步骤中，创建动画序列，并在**txt2img**和**img2img** tab中增加一个功能区域(registers a script)。  

创建动画对整体性能的影响很小，因为它不需要单独的运行，只需要增加保存每个中间步骤的图像以及实际创建movie文件的几秒钟的开销。 

支持**color**和**motion** interpolation，以从任意数量的中间步骤实现所需持续时间的动画。由于使用了优化的编解码器设置，生成的movie文件通常非常小（平均约为*1MB*）。

![screenshot](https://raw.githubusercontent.com/vladmandic/sd-extension-steps-animation/main/steps-animation.jpg)

### [Example](https://user-images.githubusercontent.com/57876960/212490617-f0444799-50e5-485e-bc5d-9c24a9146d38.mp4)


## Aesthetic Scorer
https://github.com/vladmandic/sd-extension-aesthetic-scorer

使用现有的 CLiP 模型和一个额外的小型预训练模型计算图像的感知美学分数。

通过 `Settings` -> `Aesthetic scorer`  启用或禁用。

这是一个“不可见”的扩展，它在保存任何图像之前在后台运行，并将 **`score`** 作为 *PNG info section* and/or *EXIF comments* 字段附加到图像中。

### Notes

- 通过 **Settings** &rarr; **Aesthetic scorer** 进行配置
  ![screenshot](https://raw.githubusercontent.com/vladmandic/sd-extension-aesthetic-scorer/main/aesthetic-scorer.jpg)
- 扩展遵循现有的 **Move VAE and CLiP to RAM** 设置
- 模型将在第一次使用时自动下载（small）
- 分数值为 `0..10`
- 支持 `CLiP-ViT-L/14` 和 `CLiP-ViT-B/16`
- 跨平台！

## Discord Rich Presence
https://github.com/kabachuha/discord-rpc-for-automatic1111-webui

提供与 Discord RPC 的连接，在用户配置文件中显示一个精美的表格。


## Promptgen
https://github.com/AUTOMATIC1111/stable-diffusion-webui-promptgen

Use transformers models to generate prompts.

![image](https://user-images.githubusercontent.com/98228077/223561862-27815193-acfd-47cb-ae67-fcc435b2c875.png)


## haku-img
https://github.com/KohakuBlueleaf/a1111-sd-webui-haku-img

图像工具拓展。支持：混合(blending)、分层(layering)、色调(hue)和颜色调整(color adjustments)、模糊(blurring)、草图效果(sketch effects)和基本像素化(basic pixelization)。

![image](https://user-images.githubusercontent.com/98228077/223561769-294ee4fa-f857-4dc9-afbf-dfe953e8c6ad.png)


## Merge Block Weighted
https://github.com/bbc-mc/sdweb-merge-block-weighted-gui

Merge models with separate rate for each 25 U-Net block (input, middle, output).

![image](https://user-images.githubusercontent.com/98228077/223561099-c9cb6fab-c3c6-42fb-92fd-6811474d073c.png)


## Stable Horde Worker
https://github.com/sdwebui-w-horde/sd-webui-stable-horde-worker

An unofficial [Stable Horde](https://stablehorde.net/) worker bridge as a [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) extension.

### Features

**This extension is still WORKING IN PROGRESS**，尚未准备好用于生产环境。

- 从Stable Horde获取job，生成图像并提交
- 可配置每个job之间的间隔时间
- 随时启用和禁用扩展
- 根据当前模型的推理，获取相应的job
- 在Stable Diffusion WebUI中显示生成的图像
- 将生成的图像带有 PNG 信息文本保存到本地

### Install

- 在 Stable Diffusion WebUI 安装目录下运行以下命令：

  ```bash
  git clone https://github.com/sdwebui-w-horde/sd-webui-stable-horde-worker.git extensions/stable-horde-worker
  ```

- 启动 Stable Diffusion WebUI，您将看到 `Stable Horde Worker` 选项卡。

  ![settings](./screenshots/settings.png)

- 如果您没有[Stable Horde](https://stablehorde.net/)的账户，请先注册账户并获取`API key`。

  **Note**：默认的匿名密钥 `00000000` 无效，需要使用自己的密钥。

- 在这里设置您的 `API key`。
- 在此处设置 `Worker name`，并确保它是适当的名称。
- 确保已选中 `Enable`。
- Click `Apply settings` buttons.


## Stable Horde

### Stable Horde Client
https://github.com/natanjunges/stable-diffusion-webui-stable-horde

使用其他用户的计算机生成图片。您应该能够使用匿名的 `0000000000` api key从stable horde中接收图像，但建议您获取自己的api key - https://stablehorde.net/register

注意：如果没有kudos, 检索图像可能需要>=2分钟。


## Multiple hypernetworks 
https://github.com/antis0007/sd-webui-multiple-hypernetworks

允许同时使用多个hypernetworks的扩展

![image](https://user-images.githubusercontent.com/32306715/212293588-a8b4d1e9-4099-4a2e-a61a-f549a70f6096.png) 

## Hypernetwork-Monkeypatch-Extension
https://github.com/aria1th/Hypernetwork-MonkeyPatch-Extension

这是一个提供额外训练功能的扩展，支持多个hypernetworks。

![image](https://user-images.githubusercontent.com/35677394/212069329-7f3d427f-efad-4424-8dca-4bec010ea429.png)

## Ultimate SD Upscaler
https://github.com/Coyote-A/ultimate-upscale-for-automatic1111

提供更多的SD Upscale新功能选项，比原版高降噪(0.3-0.5)生成的图片效果更好。

![image](https://user-images.githubusercontent.com/98228077/223559884-5498d495-c5f3-4068-8711-f9f31fb2d435.png)

## Model Converter
https://github.com/Akegarasu/sd-webui-model-converter

这是一个模型转换扩展，支持将fp16/bf16 no-ema/ema-only格式转换。

## Kohya-ss Additional Networks
https://github.com/kohya-ss/sd-webui-additional-networks

允许Web UI使用他们的脚本训练神经网络(LoRA)生成图像，编辑safetensors prompt和附加metadata，并使用2.X LoRAs。

![image](https://user-images.githubusercontent.com/98228077/223559083-9a5dc069-f73e-48d2-a22c-4db7b983ea40.png)

## Add image number to grid
https://github.com/AlUlkesh/sd_grid_add_image_number

在网格中添加图像的编号。

## quick-css
https://github.com/Gerschel/sd-web-ui-quickcss

快速选择和应用 custom.css 文件的扩展，用于自定义 ui 中元素的外观和位置。

## Prompt Generator
https://github.com/imrayya/stable-diffusion-webui-Prompt_Generator

在 WebUI 中添加一个选项卡，允许用户从小型基础prompt生成prompt。基于 [FredZhang7/distilgpt2-stable-diffusion-v2](https://huggingface.co/FredZhang7/distilgpt2-stable-diffusion-v2)。

## model-keyword
https://github.com/mix1009/model-keyword

自动将匹配的keyword(s)插入到prompt中。更新扩展以获取最新的model+keyword mappings。

## sd-model-preview
https://github.com/Vetchems/sd-model-preview

允许您创建一个与您的模型名称相同的txt文件和jpg/png，以便以后在webui中轻松显示此模型的图文信息。

## Enhanced-img2img
https://github.com/OedoSoldier/enhanced-img2img

支持批处理和更好的修复(support for batched and better inpainting)的扩展。有关更多详细信息[readme](https://github.com/OedoSoldier/enhanced-img2img#usage)。

## openOutpaint 扩展
https://github.com/zero01101/openOutpaint-webUI-extension

具有完整 openOutpaint UI 的选项卡。使用 --api 运行。

## Save Intermediate Images
https://github.com/AlUlkesh/sd_save_intermediate_images

实现了保存生成中间图像等更高级的功能。

## Riffusion
https://github.com/enlyth/sd-webui-riffusion

使用 Riffusion 模型在 gradio 中生成音乐。要复制 [original](https://www.riffusion.com/about) interpolation技术，请将[prompt travel extension](https://github.com/Kahsolt/stable-diffusion-webui-prompt-travel)的output frames输入到riffusion tab。

## DH Patch
https://github.com/d8ahazard/sd_auto_fix

由 D8ahazard 制作的随机补丁。自动加载 v2、2.1 模型的配置 YAML 文件；修补 latent-diffusion 以解决 2.1 模型上的注意力问题（没有 no-half 的黑盒），以及其他我想到的问题。

## Preset Utilities
https://github.com/Gerschel/sd_web_ui_preset_utils

用于 UI 的预设工具。支持一些自定义脚本的预设。

## Config-Presets
https://github.com/Zyin055/Config-Presets

添加一个可配置的下拉菜单，允许您在 txt2img 和 img2img 选项卡中更改 UI 预设设置。

## Diffusion Defender
https://github.com/WildBanjos/DiffusionDefender

Prompt黑名单、查找和替换等, for semi-private and public instances。


## NSFW checker
https://github.com/AUTOMATIC1111/stable-diffusion-webui-nsfw-censor

Replaces NSFW images with black.


## Infinity Grid Generator
https://github.com/mcmonkeyprojects/sd-infinity-grid-generator-script

构建一个包含所选参数的yaml文件，并生成无限维网格(infinite-dimensional grids)。具有向字段添加文本描述的内置功能。请参阅自述文件以获取使用详细信息。

![image](https://user-images.githubusercontent.com/98228077/208332269-88983668-ea7e-45a8-a6d5-cd7a9cb64b3a.png)


## embedding-inspector
https://github.com/tkalayci71/embedding-inspector

检查任何token(a word)或Textual-Inversion embeddings，并找出哪些embeddings是相似的。您可以在几秒钟内混合、修改或创建embeddings。自发布以来，已经发布了更多有趣的选项，详见[here](https://github.com/tkalayci71/embedding-inspector#whats-new)。

![image](https://user-images.githubusercontent.com/98228077/209546038-3f4206bf-2c43-4d58-bf83-6318ade393f4.png)


## Prompt Gallery
https://github.com/dr413677671/PromptGallery-stable-diffusion-webui

构建一个包含你的角色prompts的yaml文件，点击生成，通过它们的单词属性和修改器快速预览它们。详情请见自述文件。

![image](https://user-images.githubusercontent.com/98228077/208332199-7652146c-2428-4f44-9011-66e81bc87426.png)


## DAAM
https://github.com/toriato/stable-diffusion-webui-daam

DAAM是Diffusion Attentive Attribution Maps的缩写，输入attention文本（必须是包含在prompt中的字符串）并运行，会生成原始图像以及含有每个attention的heatmap重叠的图像。

![image](https://user-images.githubusercontent.com/98228077/208332173-ffb92131-bd02-4a07-9531-136822d06c86.png)


## Visualize Cross-Attention
https://github.com/benkyoujouzu/stable-diffusion-webui-visualize-cross-attention-extension

![image](https://user-images.githubusercontent.com/98228077/208332131-6acdae9a-2b25-4e71-8ab0-6e375c7c0419.png)

根据输入的prompts生成图像，再根据图像生成高亮选择部分。 与tokenizer扩展一起使用。


## ABG_extension
https://github.com/KutsuyaYuki/ABG_extension

自动删除背景。使用onnx model微调动漫图片. Runs on GPU.

| ![test](https://user-images.githubusercontent.com/98228077/209423352-e8ea64e7-8522-4c2c-9350-c7cd50c35c7c.png) |  ![00035-4190733039-cow](https://user-images.githubusercontent.com/98228077/209423400-720ddde9-258a-4e68-8a93-73b67dad714e.png) | ![00021-1317075604-samdoesarts portrait](https://user-images.githubusercontent.com/98228077/209423428-da7c68db-e7d1-45a1-b931-817de9233a67.png) | ![00025-2023077221-](https://user-images.githubusercontent.com/98228077/209423446-79e676ae-460e-4282-a591-b8b986bfd869.png) |
| :---: | :---: | :---: | :---: |
| ![img_-0002-3313071906-bust shot of person](https://user-images.githubusercontent.com/98228077/209423467-789c17ad-d7ed-41a9-a039-802cfcae324a.png) | ![img_-0022-4190733039-cow](https://user-images.githubusercontent.com/98228077/209423493-dcee7860-a09e-41e0-9397-715f52bdcaab.png) | ![img_-0008-1317075604-samdoesarts portrait](https://user-images.githubusercontent.com/98228077/209423521-736f33ca-aafb-4f8b-b067-14916ec6955f.png) | ![img_-0012-2023077221-](https://user-images.githubusercontent.com/98228077/209423546-31b2305a-3159-443f-8a98-4da22c29c415.png) |


## depthmap2mask
https://github.com/Extraltodeus/depthmap2mask

Create masks for img2img based on a depth estimation made by MiDaS.
使用MiDaS做的depth estimation为img2img创建mask。

![image](https://user-images.githubusercontent.com/15731540/204050868-eca8db02-2193-4115-a5b8-e8f5c796e035.png)![image](https://user-images.githubusercontent.com/15731540/204050888-41b00335-50b4-4328-8cfd-8fc5e9cec78b.png)![image](https://user-images.githubusercontent.com/15731540/204050899-8757b774-f2da-4c15-bfa7-90d8270e8287.png)


## multi-subject-render
https://github.com/Extraltodeus/multi-subject-render

这是一个depth aware扩展，可以帮助在一张图像上创建多个复杂的主体。它首先生成一个背景，然后生成多个前景主体，depth analysis后剪裁它们的背景，将它们粘贴到背景上，最后进行img2img处理以得到一个完整的结果。

![image](https://user-images.githubusercontent.com/98228077/208331952-019dfd64-182d-4695-bdb0-4367c81e4c43.png)


## Depth Maps
https://github.com/thygate/stable-diffusion-webui-depthmap-script

从生成的图像创建depthmaps。结果可以在3D或全息设备（如VR头戴式显示器或lookingglass显示器）上查看，在used in Render or Game- Engines on a plane with a displacement modifier，甚至可以进行3D打印。

![image](https://user-images.githubusercontent.com/98228077/208331747-9acba3f0-3039-485e-96ab-f0cf5619ec3b.png)


## Merge Board
https://github.com/bbc-mc/sdweb-merge-board

多进程合并支持(Multiple lane merge support)，最多同时10个进程。 Save and Load your merging combination as Recipes, which is simple text.

![image](https://user-images.githubusercontent.com/98228077/208331651-09a0d70e-1906-4f80-8bc1-faf3c0ca8fad.png)

also see:
https://github.com/Maurdekye/model-kitchen


## gelbooru-prompt
https://github.com/antis0007/sd-webui-gelbooru-prompt

根据图片hash获取tag。


## booru2prompt
https://github.com/Malisius/booru2prompt

这个 SD 扩展可以turn posts from various image boorus into stable diffusion prompts。它通过从他们的API拉取tag列表来实现。你可以自己复制粘贴想要的帖子链接，或使用内置的搜索功能，不需要离开 SD 的情况下完成所有操作。

![image](https://user-images.githubusercontent.com/98228077/208331612-dad61ef7-33dd-4008-9cc7-06b0b0a7cb6d.png)

also see:
https://github.com/stysmmaker/stable-diffusion-webui-booru-prompt


## WD 1.4 Tagger
https://github.com/toriato/stable-diffusion-webui-wd14-tagger

使用训练过的模型文件, 生产 WD 1.4 Tags. Model link - https://mega.nz/file/ptA2jSSB#G4INKHQG2x2pGAVQBn-yd_U5dMgevGF8YYM9CR_R1SY

![image](https://user-images.githubusercontent.com/98228077/208331569-2cf82c5c-f4c3-4181-84bd-2bdced0c2cff.png)


## DreamArtist
***[NEW]DreamArtist++***
DreamArtist++ 现已发布，训练lora只需1张图。

[HCP-Diffusion](https://github.com/7eu7d7/HCP-Diffusion)

新版DreamArtist++拥有之前DreamArtist的所有功能。

***[old]DreamArtist***

https://github.com/7eu7d7/DreamArtist-sd-wbui-extension

DreamArtist 只需一张训练图像，就可以学习其中的内容和风格，从而生成具有高可控性的多样化高质量图像

![image](https://user-images.githubusercontent.com/98228077/208331536-069783ae-32f7-4897-8c1b-94e0ae14f9cd.png)


## Auto TLS-HTTPS
https://github.com/papuSpartan/stable-diffusion-webui-auto-tls-https

使用https打开stable-diffusion-webui


## Randomize
~~https://github.com/stysmmaker/stable-diffusion-webui-randomize~~
fork: https://github.com/innightwolfsleep/stable-diffusion-webui-randomize

生成期间允许在 txt2img 使用随机参数。无论选择什么脚本，这个脚本总是会启用。这意味着此脚本也将与其他脚本一起运行，例如 AUTOMATIC1111/stable-diffusion-webui-wildcards。

## conditioning-highres-fix
https://github.com/klimaleksus/stable-diffusion-webui-conditioning-highres-fix

这个拓展是用来在运行时，根据Denoising strength重写Inpainting conditioning mask强度值。

这对于修复模型(Inpainting models)很有用，例如 sd-v1-5-inpainting.ckpt

![image](https://user-images.githubusercontent.com/98228077/208331374-5a271cf3-cfac-449b-9e09-c63ddc9ca03a.png)


## Detection Detailer
https://github.com/dustysys/ddetailer

Stable Diffusion web UI的一个拓展，可以推理出对象(object detection)，并自动mask.

<img src="https://github.com/dustysys/ddetailer/raw/master/misc/ddetailer_example_3.gif"/>


## Sonar
https://github.com/Kahsolt/stable-diffusion-webui-sonar

提高生成的图像质量，在某个已知图像的附近搜索相似（或更好）的图像，专注于single prompt优化而不是在多个prompts之间traveling。

![image](https://user-images.githubusercontent.com/98228077/209545702-c796a3f8-4d8c-4e2b-9b2e-920008ec2f32.png)![image](https://user-images.githubusercontent.com/98228077/209545756-31c94fec-d783-447f-8aac-4a5bba43ea15.png)


## prompt travel
https://github.com/Kahsolt/stable-diffusion-webui-prompt-travel

AUTOMATIC1111/stable-diffusion-webui 的扩展脚本，用于在latent space中的prompts之间移动(travel)。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://github.com/ClashSAN/bloated-gifs/blob/main/prompt_travel.gif" width="512" height="512" />
</details>


## shift-attention
https://github.com/yownas/shift-attention

在prompt中生成一系列图像来转移注意力。 此脚本使您能够在prompt中为tokens的权重指定一个范围，然后生成从第一个到第二个的一系列图像。

https://user-images.githubusercontent.com/13150150/193368939-c0a57440-1955-417c-898a-ccd102e207a5.mp4


## seed travel
https://github.com/yownas/seed_travel

Small script for AUTOMATIC1111/stable-diffusion-webui to create images that exists between seeds.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://github.com/ClashSAN/bloated-gifs/blob/main/seedtravel.gif" width="512" height="512" />
</details>


## Embeddings editor
https://github.com/CodeExplode/stable-diffusion-webui-embedding-editor

使用滑块修改textual inversion embeddings。

![image](https://user-images.githubusercontent.com/98228077/208331138-cdfe8f43-78f7-499e-b746-c42355ee8d6d.png)


## Latent Mirroring
https://github.com/dfaker/SD-latent-mirroring

通过对latent images的镜像和翻转，微妙平衡结构(subtle balanced compositions)，创造更好的结果(perfect reflections)。

![image](https://user-images.githubusercontent.com/98228077/208331098-3b7fefce-6d38-486d-9543-258f5b2b0fd6.png)


## StylePile
https://github.com/some9000/StylePile
			
将prompts中的元素混合和匹配，影响结果的样式的一种简单方法。

![image](https://user-images.githubusercontent.com/98228077/208331056-2956d050-a7a4-4b6f-b064-72f6a7d7ee0d.png)


## Push to 🤗 Hugging Face

https://github.com/camenduru/stable-diffusion-webui-huggingface

![Push Folder to Hugging Face](https://user-images.githubusercontent.com/54370274/206897701-9e86ce7c-af06-4d95-b9ea-385276c99d3a.jpg)

To install it, clone the repo into the `extensions` directory and restart the web ui:

`git clone https://github.com/camenduru/stable-diffusion-webui-huggingface`

`pip install huggingface-hub`


## Tokenizer
https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer

增加一个tab，预览CLIP model如何标记文字(text)

![about](https://user-images.githubusercontent.com/20920490/200113798-50b55f5a-45db-4b6f-93c0-ae6be75e5788.png)


## novelai-2-local-prompt
https://github.com/animerl/novelai-2-local-prompt

添加一个按钮，将 NovelAI 中使用的prompts转换为在 WebUI 中使用。 此外，添加一个按钮，使您可以调用以前使用过的prompt。

![pic](https://user-images.githubusercontent.com/113022648/197382468-65f4a96d-48af-4890-8fcf-0ec7c3b9ec3a.png)


## Booru tag autocompletion
https://github.com/DominikDoom/a1111-sd-webui-tagcomplete

显示来自"image booru" boards 的 tags自动完成提示，例如Danbooru。使用本地tag CSV 文件和一个自定义的配置。

![image](https://user-images.githubusercontent.com/20920490/200016417-9451efdb-5d0d-4131-bd9e-39a687be8dd7.png)


## Unprompted
https://github.com/ThereforeGames/unprompted
 
使用这种强大的脚本语言增强您的prompt工作流程！

![unprompted_header](https://user-images.githubusercontent.com/95403634/199041569-7c6c5748-e7dc-4068-943f-c2d92745dbb5.png)

**Unprompted** 是一个高度模块化的扩展程序，可用于 [AUTOMATIC1111's Stable Diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)，它允许你在prompts中包含各种简码。你可以从文件中提取文本、设置自己的变量、通过条件函数处理文本等等，就像是强化版的通配符。

虽然预期用例是 Stable Diffusion，**但这个引擎也足够灵活，可以作为通用文本生成器。**


## training-picker
https://github.com/Maurdekye/training-picker

向 webui 添加一个选项卡，允许用户从视频中自动提取关键帧，并手动提取这些帧的 512x512 裁剪以用于模型训练。

![image](https://user-images.githubusercontent.com/2313721/199614791-1f573573-a2e2-4358-836d-5655825077e1.png)

**Installation**

- 安装[AUTOMATIC1111's Stable Diffusion Webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- 为您的操作系统安装[ffmpeg](https://ffmpeg.org/)
- 将此存储库克隆到webui内部的扩展文件夹
- 将您想要从中提取裁剪帧的视频放入training-picker/videos文件夹中


## auto-sd-paint-ext

https://github.com/Interpause/auto-sd-paint-ext

>AUTOMATIC1111的webUI Krita插件扩展（其他drawing studio即将推出？）

![image](https://user-images.githubusercontent.com/98228077/217986983-d23a334d-50bc-4bb1-ac8b-60f99a3b07b9.png)

- 优化工作流程(txt2img, img2img, inpaint, upscale)和UI设计。
- 唯一公开Script API的drawing studio插件。

See https://github.com/Interpause/auto-sd-paint-ext/issues/41 for planned developments.
See [CHANGELOG.md](https://github.com/Interpause/auto-sd-paint-ext/blob/main/CHANGELOG.md) for the full changelog.


## Dataset Tag Editor
https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor

[日本語 Readme](https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor/blob/main/README-JP.md)

这是一个用于编辑[Stable Diffusion web UI by AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui)训练dataset中的标题的扩展程序。

它适用于以逗号分隔样式的文本标题（例如DeepBooru interrogator生成的tag）。

可以加载图像文件名中的标题，但编辑的标题只能以文本文件的形式保存。

![picture](https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor/blob/main/ss01.png)


## Aesthetic Image Scorer
https://github.com/tsngo/stable-diffusion-webui-aesthetic-image-scorer

Extension for https://github.com/AUTOMATIC1111/stable-diffusion-webui

使用基于[Chad Scorer](https://github.com/grexzen/SD-Chad/blob/main/chad_scorer.py)的[CLIP+MLP Aesthetic Score Predictor](https://github.com/christophschuhmann/improved-aesthetic-predictor) 给生成的图片打分。

See [Discussions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1831)

Saves score to windows tags with other options planned

![picture](https://github.com/tsngo/stable-diffusion-webui-aesthetic-image-scorer/blob/main/tag_group_by.png)


## Artists to study
https://github.com/camenduru/stable-diffusion-webui-artists-to-study

https://artiststostudy.pages.dev/ adapted to an extension for [web ui](https://github.com/AUTOMATIC1111/stable-diffusion-webui).

To install it, clone the repo into the `extensions` directory and restart the web ui:

`git clone https://github.com/camenduru/stable-diffusion-webui-artists-to-study`

You can add the artist name to the clipboard by clicking on it. (thanks for the idea @gmaciocci)

![picture](https://user-images.githubusercontent.com/54370274/197829512-e7d30d44-2697-4ecd-b9a7-3665217918c7.jpg)


## Deforum
https://github.com/deforum-art/deforum-for-automatic1111-webui

Deforum 的官方端口，一个用于 2D 和 3D 动画的扩展脚本，支持关键帧序列、动态数学参数（甚至在prompts内）、动态遮罩、深度估计和变形。

![image](https://user-images.githubusercontent.com/98228077/217986819-67fd1e3c-b007-475c-b8c9-3bf28ee3aa67.png)


## Inspiration(灵感)
https://github.com/yfszzx/stable-diffusion-webui-inspiration

随机展示艺术家或艺术流派典型风格的图片，在选择后会显示更多该艺术家或流派的图片。因此，当您创作时无需担心选择正确的艺术风格有多么困难。

![68747470733a2f2f73362e6a70672e636d2f323032322f31302f32322f504a596f4e4c2e706e67](https://user-images.githubusercontent.com/20920490/197518700-3f753132-8799-4ad0-8cdf-bcdcbf7798aa.png)


## Image Browser
https://github.com/AlUlkesh/stable-diffusion-webui-images-browser

提供在网络浏览器中浏览创建的图像的界面，允许按 EXIF 数据进行排序和过滤。

![image](https://user-images.githubusercontent.com/23466035/217083703-0845da05-3305-4f5a-af53-f3829de6a29d.png)


## Smart Process
https://github.com/d8ahazard/sd_smartprocess

智能裁剪、字幕和图像增强。

![image](https://user-images.githubusercontent.com/1633844/201435094-433d765c-56e8-4573-82d9-71af2b112159.png)


## Dreambooth
https://github.com/d8ahazard/sd_dreambooth_extension

在UI中加入Dreambooth。有关配置要求，请参阅项目readme。包含LoRA(Low Rank Adaptation)。

基于ShivamShiaro的仓库。

![image](https://user-images.githubusercontent.com/1633844/201434706-2c2744ba-082e-427e-9f8d-af03de204583.png)


## Dynamic Prompts
https://github.com/adieyal/sd-dynamic-prompts

这是一个为[AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)定制的扩展，它实现了一个表达丰富的模板语言，用于随机或组合式的prompt生成，并支持深层通配符目录结构。

在[readme](https://github.com/adieyal/sd-dynamic-prompts)中，还展示了更多的功能和扩展。

![image](https://github.com/adieyal/sd-dynamic-prompts/raw/main/images/extension.png)

使用此扩展，例如以下提示：

`A {house|apartment|lodge|cottage} in {summer|winter|autumn|spring} by {2$$artist1|artist2|artist3}`

Will any of the following prompts:

- A house in summer by artist1, artist2
- A lodge in autumn by artist3, artist1
- A cottage in winter by artist2, artist3
- ...

如果你正在寻找有趣的艺术和风格组合，这将特别有用。

你也可以从文件中随机选择一个字符串。假设你在WILDCARD_DIR中有一个名为seasons.txt的文件，那么：

`__seasons__ is coming`

将会生成如下prompts:

- Winter is coming
- Spring is coming
- ...

You can also use the same wildcard twice

`I love __seasons__ better than __seasons__`

- I love Winter better than Summer
- I love Spring better than Spring

## Wildcards
https://github.com/AUTOMATIC1111/stable-diffusion-webui-wildcards

允许您在prompt中使用`__name__`语法从wildcard目录中的名为`name.txt`的文件中获取随机行。

## Aesthetic Gradients
https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients

从一张或几张图片中创建一个embedding，然后使用它将它们的风格应用于生成的图像。

![firefox_FgKg9dx9eF](https://user-images.githubusercontent.com/20920490/197466300-6b042bcf-5cba-4600-97d7-ad2652875706.png)

## 3D Model&Pose Loader
https://github.com/jtydhr88/sd-3dmodel-loader

一种自定义扩展，允许您在webui中加载本地的3D模型/动画，或进行姿势编辑，然后将截屏发送给txt2img或img2img作为您的ControlNet的参考图像。

![1](https://user-images.githubusercontent.com/860985/236643711-140579dc-bb74-4a84-ba40-3a64823285ab.png)

## Canvas Editor
https://github.com/jtydhr88/sd-canvas-editor

一种为SD-WebUI定制的扩展，集成了全能canvas编辑器，您可以在其中使用图层(layer)、文本、图像、元素(elements)等功能。

![overall](https://user-images.githubusercontent.com/860985/236643824-946ebdef-83af-4e85-80ea-6ac7d2fb520e.png)


## One Button Prompt
https://github.com/AIrjen/OneButtonPrompt

One Button Prompt是automatic1111的工具/脚本，适用于在编写良好prompt时遇到问题的初学者，或想要获得灵感的高级用户。

它从scratch生成整个prompt。它是随机的，但受到控制。您只需加载脚本并按生成。

![One Button Prompt](https://user-images.githubusercontent.com/40751091/238175610-9491d781-c0ad-4f22-93c4-b361635fc265.jpg)


## Model Downloader
https://github.com/Iyashinouta/sd-model-downloader

从CivitAI和HuggingFace下载模型的SD-Webui扩展，推荐给Cloud Users(a.k.a Google Colab, etc.)

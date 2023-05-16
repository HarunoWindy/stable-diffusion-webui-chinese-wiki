# Installing and Using Custom Scripts
要安装自定义脚本，请将它们放入`scripts`目录中，然后在设置选项卡底部单击`Reload custom script`按钮。安装后，自定义脚本将出现在txt2img和img2img选项卡左下角的下拉菜单中。以下是一些由Web UI用户创建的值得注意的自定义脚本：


## txt2img2img 
https://github.com/ThereforeGames/txt2img2img

极大地提高了任何角色/主题的可编辑性，同时保留了它们的相似性。这个脚本的主要动机是通过[Textual Inversion](https://textual-inversion.github.io/)改进embeddings的可编辑性。

（克隆时要小心，因为它检查了一点venv）

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/98228077/200106431-21a22657-db24-4e9c-b7fa-e3a8e9096b89.png" width="624" height="312" />
</details>

## txt2mask
https://github.com/ThereforeGames/txt2mask

允许您使用文本指定修复掩码，而不是画笔。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/95403634/190878562-d020887c-ccb0-411c-ab37-38e2115552eb.png" width="674" height="312" />
</details>

## Mask drawing UI
https://github.com/dfaker/stable-diffusion-webui-cv2-external-masking-script

提供一个由CV2支持的本地弹出窗口，在处理前允许添加掩码。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/98228077/200109495-3d6741f1-0e25-4ae5-9f84-d93f886f302a.png" width="302" height="312" />
</details>

## Img2img Video
https://github.com/memes-forever/Stable-diffusion-webui-video

使用img2img，一个接一个地生成图片。

## Advanced Seed Blending
https://github.com/amotile/stable-diffusion-backend/tree/master/src/process/implementations/automatic1111_scripts

这个脚本允许您基于多个加权种子(seed)来设置初始噪声。

例如：`seed1:2, seed2:1, seed3:1`

权重是归一化的，所以您可以使用更大的数字，如上面的例子，或者您可以使用浮点数：

Ex. `seed1:0.5, seed2:0.25, seed3:0.25`

## Prompt Blending
https://github.com/amotile/stable-diffusion-backend/tree/master/src/process/implementations/automatic1111_scripts

这个脚本允许您在生成图像之前，通过数学地组合它们的文本嵌入来将多个加权提示(prompt)组合在一起。

例如：

`Crystal containing elemental {fire|ice}`

它支持嵌套定义，所以您也可以这样做：

`Crystal containing elemental {{fire:5|ice}|earth}`

## Animator
https://github.com/Animator-Anon/Animator

一个基础的img2img脚本，它将转储帧并构建视频文件。适用于创建有趣的缩放变形电影，但目前还没有太多其他用途。

## Parameter Sequencer
https://github.com/rewbs/sd-parseq

使用严格控制和灵活插值生成视频，可以控制许多Stable Diffusion参数（如种子(seed)，比例(scale)，提示(prompt)权重，降噪强度(Denoising strength)等），以及输入处理参数（如缩放，平移，3D旋转等）。

## Alternate Noise Schedules
https://gist.github.com/dfaker/f88aa62e3a14b559fe4e5f6b345db664

使用替代生成器来生成采样器的sigma时间表。

允许访问crowsonkb/k-diffusion中的Karras，指数和方差保持时间表及其参数。

## Vid2Vid
https://github.com/Filarius/stable-diffusion-webui/blob/master/scripts/vid2vid.py

从真实视频中，将帧img2img并将它们拼接在一起。不会将帧解压到硬盘驱动器。

## Txt2VectorGraphics
https://github.com/GeorgLegato/Txt2Vectorgraphics

从您的提示(prompt)中创建自定义、可缩放的图标，以SVG或PDF格式输出。

<details><summary>Example: (Click to expand:)</summary>

| prompt  |PNG  |SVG |
| :--------  | :-----------------: | :---------------------: |
| Happy Einstein | <img src="https://user-images.githubusercontent.com/7210708/193370360-506eb6b5-4fa7-4b2a-9fec-6430f6d027f5.png" width="40%" /> | <img src="https://user-images.githubusercontent.com/7210708/193370379-2680aa2a-f460-44e7-9c4e-592cf096de71.svg" width=30%/> |
| Mountainbike Downhill | <img src="https://user-images.githubusercontent.com/7210708/193371353-f0f5ff6f-12f7-423b-a481-f9bd119631dd.png" width=40%/> | <img src="https://user-images.githubusercontent.com/7210708/193371585-68dea4ca-6c1a-4d31-965d-c1b5f145bb6f.svg" width=30%/> |
coffe mug in shape of a heart | <img src="https://user-images.githubusercontent.com/7210708/193374299-98379ca1-3106-4ceb-bcd3-fa129e30817a.png" width=40%/> | <img src="https://user-images.githubusercontent.com/7210708/193374525-460395af-9588-476e-bcf6-6a8ad426be8e.svg" width=30%/> |
| Headphones | <img src="https://user-images.githubusercontent.com/7210708/193376238-5c4d4a8f-1f06-4ba4-b780-d2fa2e794eda.png" width=40%/> | <img src="https://user-images.githubusercontent.com/7210708/193376255-80e25271-6313-4bff-a98e-ba3ae48538ca.svg" width=30%/> |

</details>

## Loopback and Superimpose
https://github.com/DiceOwl/StableDiffusionStuff

https://github.com/DiceOwl/StableDiffusionStuff/blob/main/loopback_superimpose.py

将img2img的输出与原始输入图像以alpha强度混合。结果再次输入到img2img中（在loop>=2时），并重复此过程。倾向于使图像更清晰，提高一致性，减少创造力和减少细节。

## Interpolate
https://github.com/DiceOwl/StableDiffusionStuff

https://github.com/DiceOwl/StableDiffusionStuff/blob/main/interpolate.py

一个img2img脚本，用于生成中间图像。允许两个输入图像进行插值。更多功能请参见[readme](https://github.com/DiceOwl/StableDiffusionStuff)。

## Run n times
https://gist.github.com/camenduru/9ec5f8141db9902e375967e93250860f

Run n times with random seed.

## Advanced Loopback
https://github.com/Extraltodeus/advanced-loopback-for-sd-webui

具有参数变化和提示(prompt)切换等其他功能的动态缩放回路！

## prompt-morph
https://github.com/feffy380/prompt-morph

使用Stable Diffusion生成形态序列。 在两个或多个提示(prompt)之间插值并在每个步骤(step)创建图像(image)。

使用新的AND关键字，并可以选择将序列作为视频导出。

## prompt interpolation
https://github.com/EugeoSynthesisThirtyTwo/prompt-interpolation-script-for-sd-webui

使用此脚本，您可以在两个提示(prompt)之间插值（使用“AND”关键字），生成您想要的任意数量的图像(image)。
您还可以使用结果生成gif。 适用于txt2img和img2img。

<details><summary>Example: (Click to expand:)</summary>

![gif](https://user-images.githubusercontent.com/24735555/195470874-afc3dfdc-7b35-4b23-9c34-5888a4100ac1.gif)

</details>

## Asymmetric Tiling
https://github.com/tjm35/asymmetric-tiling-sd-webui/

独立控制水平/垂直无缝平铺(tile)。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/19196175/195132862-8c050327-92f3-44a4-9c02-0f11cce0b609.png" width="624" height="312" />
</details>

## Force Symmetry
https://gist.github.com/1ort/2fe6214cf1abe4c07087aac8d91d0d8a

see https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2441

每n个步骤(step)将对称性应用于图像(image)，并将结果进一步发送到img2img。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/83316072/196016119-0a03664b-c3e4-49f0-81ac-a9e719b24bd1.png" width="624" height="312" />
</details>

## txt2palette
https://github.com/1ort/txt2palette

通过文本描述生成调色板。 此脚本获取生成的图像(image)，并将它们转换为调色板。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/83316072/199360686-62f0f5ec-ed3d-4c0f-95b4-af9c67d1e248.png" width="352" height="312" />
</details>

## XYZ Plot Script
https://github.com/xrpgame/xyz_plot_script

生成.html文件以交互式浏览图像集(imageset)。 使用滚轮或箭头键在Z维度中移动。

<details><summary>Example: (Click to expand:)</summary>
<img src="https://raw.githubusercontent.com/xrpgame/xyz_plot_script/master/example1.png" width="522" height="312" />
</details>

## Expanded-XY-grid
https://github.com/0xALIVEBEEF/Expanded-XY-grid

为AUTOMATIC1111的stable-diffusion-webui添加了更多功能的自定义脚本，以标准xy网格为基础：

- 多工具：允许在一个轴上使用多个参数，理论上允许在一个xy网格中调整无限个参数

- 可定制的提示(prompt)矩阵

- 在目录中分组文件

- S/R占位符 - 用所需值替换占位符值（参数列表中的第一个值）。

- 将PNGinfo添加到网格图像

<details><summary>Example: (Click to expand:)</summary>

<img src="https://user-images.githubusercontent.com/80003301/202277871-a4a3341b-13f7-42f4-a3e6-ca8f8cd8250a.png" width="574" height="197" />

Example images: Prompt: "darth vader riding a bicycle, modifier"; X: Multitool: "Prompt S/R: bicycle, motorcycle | CFG scale: 7.5, 10 | Prompt S/R Placeholder: modifier, 4k, artstation"; Y: Multitool: "Sampler: Euler, Euler a | Steps: 20, 50" 

</details>

## Embedding to PNG
https://github.com/dfaker/embedding-to-png-script

Converts existing embeddings to the shareable image versions.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/35278260/196052398-268a3a3e-0fad-46cd-b37d-9808480ceb18.png" width="263" height="256" />
</details>

## Alpha Canvas
https://github.com/TKoestlerx/sdexperiments

Outpaint a region. Infinite outpainting concept, used the two existing outpainting scripts from the AUTOMATIC1111 repo as a basis.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/86352149/199517938-3430170b-adca-487c-992b-eb89b3b63681.jpg" width="446" height="312" />
</details>

## Random grid
https://github.com/lilly1987/AI-WEBUI-scripts-Random

Randomly enter xy grid values.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/20321215/197346726-f93b7e84-f808-4167-9969-dc42763eeff1.png" width="198" height="312" />

基本逻辑与x/y图相同，只是在内部，x类型固定为步骤(step)，y类型固定为cfg。
在步骤(step)1|2值（10-30）的范围内生成与步骤(step)计数（10）一样多的x值
在cfg1|2值（6-15）的范围内生成与cfg计数（10）一样多的x值
即使您将1|2范围上限倒置，它也会自动更改。
对于cfg值，它被视为int类型，不读取小数值。
</details>

## Random
https://github.com/lilly1987/AI-WEBUI-scripts-Random

Repeat a simple number of times without a grid.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/20321215/197346617-0ed1cd09-0ddd-48ad-8161-bc1540d628ad.png" width="258" height="312" />
</details>

## Stable Diffusion Aesthetic Scorer
https://github.com/grexzen/SD-Chad

Rates your images.

## img2tiles
https://github.com/arcanite24/img2tiles

generate tiles from a base image. Based on SD upscale script.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://github.com/arcanite24/img2tiles/raw/master/examples/example5.png" width="312" height="312" />
</details>

## img2mosiac
https://github.com/1ort/img2mosaic

Generate mosaics from images. The script cuts the image into tiles and processes each tile separately. The size of each tile is chosen randomly.

<details><summary>Example: (Click to expand:)</summary>
<img src="https://user-images.githubusercontent.com/83316072/200170569-0e7131e4-1da8-4caf-9cd9-5b785c9d21b0.png" width="758" height="312" />
</details>

## Test my prompt
https://github.com/Extraltodeus/test_my_prompt

您是否曾经使用过一个非常长的提示(prompt)，其中充满了您不确定是否对图像(image)产生实际影响的单词？ 您是否失去了勇气逐一尝试删除它们以测试它们的效果是否值得您珍贵的GPU？

现在，您不再需要任何勇气，因为这个脚本是为您量身定做的！

它生成与提示(prompt)中单词数量相同的图像(image)（当然，您可以选择分隔符）。

<details><summary>Example: (Click to expand:)</summary>

Here the prompt is simply : "**banana, on fire, snow**" and so as you can see it has generated each image without each description in it.

<img src="https://user-images.githubusercontent.com/15731540/200349119-e45d3cfb-39f0-4999-a8f0-4671a6393824.png" width="512" height="512" />

You can also test your negative prompt.

</details>

## Pixel Art
https://github.com/C10udburst/stable-diffusion-webui-scripts

Simple script which resizes images by a variable amount, also converts image to use a color palette of a given size.

<details><summary>Example: (Click to expand:)</summary>

| Disabled | Enabled x8, no resize back, no color palette | Enabled x8, no color palette | Enabled x8, 16 color palette |
| :---: | :---: | :---: | :---: |
|![preview](https://user-images.githubusercontent.com/18114966/201491785-e30cfa9d-c850-4853-98b8-11db8de78c8d.png) | ![preview](https://user-images.githubusercontent.com/18114966/201492204-f4303694-e98d-4ea3-8256-538a88ea26b6.png) | ![preview](https://user-images.githubusercontent.com/18114966/201491864-d0c0c9f1-e34f-4cb6-a68e-7043ec5ce74e.png) | ![preview](https://user-images.githubusercontent.com/18114966/201492175-c55fa260-a17d-47c9-a919-9116e1caa8fe.png) |

[model used](https://publicprompts.art/all-in-one-pixel-art-dreambooth-model/)
```text
japanese pagoda with blossoming cherry trees, full body game asset, in pixelsprite style
Steps: 20, Sampler: DDIM, CFG scale: 7, Seed: 4288895889, Size: 512x512, Model hash: 916ea38c, Batch size: 4
```

</details>

## Scripts by FartyPants
https://github.com/FartyPants/sd_web_ui_scripts

### Hallucinate

- swaps negative and positive prompts

### Mr. Negativity

- 更高级的脚本，根据Mr. negativity rage交换负面(negative)和正面(positive)令牌。

## gif2gif
https://github.com/LonicaMewinsky/gif2gif

此脚本的目的是接受动画gif作为输入，像img2img通常那样处理帧，然后将它们重新组合成动画gif。 没有打算具有广泛的功能。 参考了prompts_from_file的代码。

## Post-Face-Restore-Again
https://github.com/butaixianran/Stable-Diffusion-Webui-Post-Face-Restore-Again

Run face restore twice in one go, from extras tab.

## Infinite Zoom
https://github.com/coolzilj/infinite-zoom

Generate Zoom in/out videos, with outpainting, as a custom script for inpaint mode in img2img tab.

## ImageReward Scorer

https://github.com/THUDM/ImageReward#integration-into-stable-diffusion-web-ui

An image **scorer** based on [ImageReward](https://github.com/THUDM/ImageReward), the first general-purpose text-to-image human preference RM, which is trained on in total **137k pairs of expert comparisons**.

[**Features**](https://github.com/THUDM/ImageReward#features) developed to date (2023-04-24) include:  (click to expand demo video)
<details>
    <summary>1. Score generated images and append to image information</summary>
    
https://user-images.githubusercontent.com/98524878/233889441-d593675a-dff4-43aa-ad6b-48cc68326fb0.mp4
  
</details>
<details>
    <summary>2. Automatically filter out images with low scores</summary>
    
https://user-images.githubusercontent.com/98524878/233889490-5c4a062f-bb5e-4179-ba98-b336cda4d290.mp4
  
</details>

For details including **installing** and **feature-specific usage**, check [the script introduction](https://github.com/THUDM/ImageReward#integration-into-stable-diffusion-web-ui).


## Saving steps of the sampling process
(Example Script) \
This script will save steps of the sampling process to a directory.
```python
import os.path

import modules.scripts as scripts
import gradio as gr

from modules import shared, sd_samplers_common
from modules.processing import Processed, process_images

class Script(scripts.Script):
    def title(self):
        return "Save steps of the sampling process to files"

    def ui(self, is_img2img):
        path = gr.Textbox(label="Save images to path", placeholder="Enter folder path here. Defaults to webui's root folder")
        return [path]

    def run(self, p, path):
        if not os.path.exists(path):
            os.makedirs(path)
        index = [0]

        def store_latent(x):
            image = shared.state.current_image = sd_samplers_common.sample_to_image(x)
            image.save(os.path.join(path, f"sample-{index[0]:05}.png"))
            index[0] += 1
            fun(x)

        fun = sd_samplers_common.store_latent
        sd_samplers_common.store_latent = store_latent

        try:
            proc = process_images(p)
        finally:
            sd_samplers_common.store_latent = fun

        return Processed(p, proc.images, p.seed, "")
```

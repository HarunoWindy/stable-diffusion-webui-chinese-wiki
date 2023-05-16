## [2023-04-29](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/9669) - Fix prompt schedule for second order samplers
第二阶采样器(Heun，DPM2/a，DPM++ 2S/a，DPM++ SDE / Karras)会导致提示计划运行速度加快两倍，例如 `[dog:cat:0.5]` (即对于100个步骤，提示为`dog`直到步骤25，`cat`直到50，并保持`dog`直到100)。这通过检查采样器是否为这些第二阶采样器中的任何一个并将步骤计数乘以2来计算提示计划来修复。

# [2023-03-26](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/80b26d2a69617b75d2d01c1e6b7d11445815ed4d) - Apply LoRA by altering layer's weights
简而言之(Too Long; Didn’t Read)：生成的图片有一点不同。如果使用高分辨率修复，这些小差异可能会被放大成大的差异。

80b26d2a69617b75d2d01c1e6b7d11445815ed4d中引入的新方法允许一次预先计算新的模型(model)权重，然后在创建图像时不必再做任何事情。有了这个，添加许多loras会在第一次应用这些loras时产生小的性能开销，之后将像没有启用任何loras一样快。旧方法会因为每增加一个新的lora而大大降低生成速度。

生成的图像之间的差异很小，但如果这对您(或您正在使用的某个扩展)很重要，1.2.0添加了一个选项来使用旧方法。

## [2023-02-18](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/a77ac2eeaad82dcf71edc6770ae82745b7d55423) - deterministic DPM++ SDE across different batch sizes
DPM++ SDE和DPM++ SDE Karras采样器在批量生成图像时与单个图像具有相同的参数，但产生不同的图像。PR https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/7730 修复了这个问题。但是修复的性质也改变了单个图像的生成方式。在兼容性设置中添加了一个选项来恢复旧的行为：不要使DPM++ SDE在不同批次大小之间确定性。

## [2023-01-11](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/035f2af050da98a8b3f847624ef3b5bc3395e87e) - Alternating words syntax bugfix
如果您在[97ff69ef](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/97ff69eff338c6641f4abf430bf5ac112c1775e0)之前使用了带有强调的交替单词语法修复程序，程序会错误地将强调部分替换为仅为`(`。因此，`[a|(b:1.1)]`，而不是变成一个序列

`a` -> `(b:1.1)` -> `a` -> `(b:1.1)` -> ...

becomes

`a` -> `(` -> `a` -> `(` -> ...

这个错误已经被修复了。如果您需要重现旧的种子(seed)，请自行将左括号放入您的提示中(`[a|\(]`)。

## [2023-01-05](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/6044) - Karras sigma min/max
一些讨论在这里：[PR](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4373)

要恢复旧的sigmas (0.1到10)，请使用设置：`Use old karras scheduler sigmas`。

## [2023-01-02](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/ef27a18b6b7cb1a8eebdc9b2e88d25baf2c2414d) - Hires fix rework
不再使用宽度/高度来指定目标分辨率，而是使用宽度/高度来指定第一次通过的分辨率，然后使用“按比例缩放”乘数(Hires upscale)或直接使用“调整宽度至”和/或“调整高度至”(Hires resize)来设置结果分辨率。

以下是旧设置和新设置之间的对应关系：

| Old version                               | New version                                                                                     |
|-------------------------------------------|-------------------------------------------------------------------------------------------------|
| Size: 1024x1024                           | Size: 512x512, Hires upscale: 2.0                                                               |
| Size: 1280x1024, First pass size: 640x512 | Size: 640x512, Hires upscale: 2.0; Alternatively Size: 640x512, Hires resize: 1280x1024                                                               |
| Size: 1024x1280, First pass size: 0x0     | Size: 512x576 (auto-calcualted if you use old infotext - paste it into prompt and use ↙️ button), Hires upscale: 2.0                     |
| Size: 1024x512, First pass size: 512x512  | Size: 512x512, Hires resize: 1024x512 |

## [2022-09-29](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/c1c27dad3ba371a5ae344b267c760aa51e77f193) - New emphasis implementation
新的实现支持转义字符和数值权重。新实现的一个缺点是旧的实现并不完美，有时会吃掉字符：例如，"a (((farm))), daytime"会变成"a farm daytime"，没有逗号。新的实现不具有这种行为，它正确地保留了所有文本，这意味着您保存的种子(seed)可能会产生不同的图片。

目前，在设置中有一个选项可以使用旧的实现：`Use old emphasis implementation`。

关于此功能的更多信息： [Attention/emphasis](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

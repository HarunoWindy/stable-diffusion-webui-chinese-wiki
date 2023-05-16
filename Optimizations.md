A number of optimization can be enabled by [commandline arguments](Command-Line-Arguments-And-Settings):

| commandline argument           | explanation                                                                                                                                                                                                                                                                                                                                                                                                                          |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--opt-sdp-attention` | 比使用xformers更快的速度，仅适用于手动安装torch 2.0到其venv的用户。(non-deterministic)
| `--opt-sdp-no-mem-attention` | 比使用xformers更快的速度，仅适用于手动安装torch 2.0到其venv的用户。（确定性，略慢于`--opt-sdp-attention`）
| `--xformers` | 使用[xformers](https://github.com/facebookresearch/xformers)库。对内存消耗和速度有很大改进。仅在少数配置上启用，因为我们只有这些配置的二进制文件。[Documentation](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers) |
| `--force-enable-xformers` | 无论程序是否认为您可以运行它，都启用上面的xformers。不要报告您运行此命令时遇到的错误。 |
| `--opt-split-attention` | 交叉注意力层优化，显著减少内存使用量，几乎没有成本（有些人报告说使用它性能更好）。黑魔法。<br/>默认情况下对`torch.cuda`启用，包括NVidia和AMD卡。 |
| `--disable-opt-split-attention` | 禁用上面的优化。 |
| `--opt-sub-quad-attention` | 子二次注意力，一种内存高效的交叉注意力层优化，可以显著减少所需内存，有时会略微降低性能。如果在xformers不起作用的硬件/软件配置下性能不佳或生成失败，则建议使用。在macOS上，这也将允许生成更大的图像。 |
| `--opt-split-attention-v1` | 使用上面优化的旧版本，不那么占用内存（它将使用更少的VRAM，但会更限制您可以制作的图片的最大尺寸）。 |
| `--medvram` | 通过将Stable Diffusion模型分成三部分 - cond（将文本转换为数值表示），first_stage（将图片转换为潜在空间并返回）和unet（实际去噪潜在空间）并使得一次只有一个在VRAM中，其他发送到CPU RAM中，使Stable Diffusion模型消耗更少的VRAM。降低性能，但只是一点点 - 除非启用实时预览。 |
| `--lowvram` | 上述优化更彻底，将unet分成多个模块，并且只保留一个模块在VRAM中。对性能造成毁灭性打击。 |
| `*do-not-batch-cond-uncond` | 在采样过程中防止正负提示批处理，这基本上可以让您以0.5批处理大小运行，节省大量内存。降低性能。不是命令行选项，而是通过使用`--medvram`或`--lowvram`隐式启用的优化。 |
| `--always-batch-cond-uncond` | 禁用上面的优化。仅与`--medvram`或`--lowvram`一起使用才有意义 |
| `--opt-channelslast` | 将稳定扩散的torch内存类型更改为最后一个通道。效果未经仔细研究。 |
| `--upcast-sampling` | 对于通常被迫使用`--no-half`运行的Nvidia和AMD卡，[should improve generation speed](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/8782)。


额外提示（Windows）：
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889 禁用硬件GPU调度。
- 禁用浏览器硬件加速
- 进入nvidia控制面板，3D参数，将电源配置文件更改为“最大性能”

## Memory & Performance Impact of Optimizers and Flags

*这是一个使用特定硬件和配置的示例测试，您的实际情况可能会有所不同*
*使用nVidia RTX3060和CUDA 11.7进行测试*

| Cross-attention | Peak Memory at Batch size 1/2/4/8/16 | Initial It/s | Peak It/s | Note |
| --------------- | ------------------------------------ | -------- | --------- | ---- |
| None            | 4.1 / 6.2 / OOM / OOM / OOM | 4.2 | 4.6 | slow and early out-of-memory
| v1              | 2.8 / 2.8 / 2.8 / 3.1 / 4.1 | 4.1 | 4.7 | 速度慢，但内存使用量最低，不需要有时会出问题的xformers
| InvokeAI        | 3.1 / 4.2 / 6.3 / 6.6 / 7.0 | 5.5 | 6.6 | almost identical to default optimizer
| Doggetx         | 3.1 / 4.2 / 6.3 / 6.6 / 7.1 | 5.4 | 6.6 | default |
| Doggetx         | 2.2 / 2.7 / 3.8 / 5.9 / 6.2 | 4.1 | 6.3 | 使用`medvram`预设可以在不大幅影响性能的情况下节省内存
| Doggetx         | 0.9 / 1.1 / 2.2 / 4.3 / 6.4 | 1.0 | 6.3 | 使用`lowvram`预设由于不断交换而极其缓慢
| Xformers        | 2.8 / 2.8 / 2.8 / 3.1 / 4.1 | 6.5 | 7.5 | fastest and low memory
| Xformers        | 2.9 / 2.9 / 2.9 / 3.6 / 4.1 | 6.4 | 7.6 | with `cuda_alloc_conf` and `opt-channelslast`

注意事项：
- 批量大小为1时的性能约为峰值性能的**~70%**
- 峰值性能通常在批量大小为8时达到
  在此之后，如果您有额外的VRAM，它会增长几个百分点，然后由于GC启动而开始下降
- 使用`lowvram`预设在批量大小低于8时性能非常低，而且到那时内存节省也不大

其他可能的优化：
- 在`webui-user.bat`中添加`set PYTORCH_CUDA_ALLOC_CONF=garbage_collection_threshold:0.9,max_split_size_mb:512`
  没有性能影响，初始内存占用略有增加，但减少了长时间运行中的内存碎片
- `opt-channelslast`
  成败参半：似乎在较高批量大小下有额外的轻微性能提升，在小尺寸下更慢，但差异在误差范围内
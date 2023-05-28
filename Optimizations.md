A number of optimization can be enabled by [commandline arguments](Command-Line-Arguments-And-Settings):

| commandline argument           | explanation                                                                                                                                                                                                                                                                                                                                                                                                                          |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--opt-sdp-attention` | 比使用xformers更快的速度，仅适用于手动安装torch 2.0到其venv的用户。(不确定的)
| `--opt-sdp-no-mem-attention` | 比使用xformers更快的速度，仅适用于手动安装torch 2.0到其venv的用户。（确定的，略慢于`--opt-sdp-attention`）
| `--xformers` | 使用[xformers库](https://github.com/facebookresearch/xformers)。减少内存消耗，提速。仅在少数配置上启用，因为我们只有这些配置的二进制文件。[Documentation](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers) |
| `--force-enable-xformers` | 强制开启xformers。无论是否可以运行它，不报告运行此命令时遇到的错误。 |
| `--opt-split-attention` | Cross attention layer优化，显著减少内存使用量，几乎没有成本（部分人在使用时性能更好）。黑魔法。<br/>默认情况下对支持NVidia和AMD卡的`torch.cuda`启用。 |
| `--disable-opt-split-attention` | 禁用上面的优化。 |
| `--opt-sub-quad-attention` | Sub-quadratic attention，一种内存高效的Cross Attention layer优化，可以显著减少所需内存，有时会略微降低性能。在不支持xformers的低配硬软件环境性能不佳或生成失败时建议使用。在macOS上，开启将可以生成更大的图像。 |
| `--opt-split-attention-v1` | 使用上面优化的旧版本，不那么占用内存（通过限制生产图片的最大尺寸减少VRAM使用）。 |
| `--medvram` | 将Stable Diffusion模型分成三部分，每次只有一部分进入VRAM，其他的部分进入CPU RAM，以消耗更少的VRAM。在不开启实时预览时只会略微降低性能。<br/>cond：将文本转换为数值表示；<br/>first_stage：将图片转换为latent space并返回；<br/>unet：latent space的真实降噪(actual denoising)）并使得一次只有一个在VRAM中，其他发送到CPU RAM中。降低性能，但只是一点点 - 除非启用实时预览。 |
| `--lowvram` | 比`--medvram`优化更彻底，将unet分成更多个模块，并且只保留一个模块在VRAM中。性能将会很差。 |
| `*do-not-batch-cond-uncond` | 在采样时，不分批处理prompts & negative prompts，这基本上可以以0.5 batch size运行，节省大量内存。但降低性能。这不是命令行选项，而是通过使用`--medvram`或`--lowvram`隐式启用的优化。 |
| `--always-batch-cond-uncond` | 禁用上面的优化。仅与`--medvram`或`--lowvram`一起使用才有意义 |
| `--opt-channelslast` | 将stable diffusion的torch内存类型更改为channels-last内存布局格式。效果未经仔细研究。 |
| `--upcast-sampling` | 对于通常强制使用`--no-half`运行的Nvidia和AMD卡，[should improve generation speed](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/8782)。


额外提示（Windows）：
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889 禁用硬件GPU调度。
- 禁用浏览器硬件加速
- 进入nvidia控制面板，3D参数，将电源配置文件更改为“最大性能”

## Memory & Performance Impact of Optimizers and Flags

*这是一个使用特定硬件&配置的示例测试，可能与您的实际情况有所不同*
*使用nVidia RTX3060和CUDA 11.7进行测试*

| Cross-attention | Peak Memory at Batch size 1/2/4/8/16 | Initial It/s | Peak It/s | Note |
| --------------- | ------------------------------------ | -------- | --------- | ---- |
| None            | 4.1 / 6.2 / OOM / OOM / OOM | 4.2 | 4.6 | slow and early out-of-memory
| v1              | 2.8 / 2.8 / 2.8 / 3.1 / 4.1 | 4.1 | 4.7 | 速度慢，但内存使用量最低，不需引入不稳定的xformers
| InvokeAI        | 3.1 / 4.2 / 6.3 / 6.6 / 7.0 | 5.5 | 6.6 | 几乎与默认优化器相同
| Doggetx         | 3.1 / 4.2 / 6.3 / 6.6 / 7.1 | 5.4 | 6.6 | default |
| Doggetx         | 2.2 / 2.7 / 3.8 / 5.9 / 6.2 | 4.1 | 6.3 | 使用`medvram`预设可以在不大幅影响性能的情况下节省内存
| Doggetx         | 0.9 / 1.1 / 2.2 / 4.3 / 6.4 | 1.0 | 6.3 | 使用`lowvram`预设由于不断交换而极其缓慢
| Xformers        | 2.8 / 2.8 / 2.8 / 3.1 / 4.1 | 6.5 | 7.5 | fastest and low memory
| Xformers        | 2.9 / 2.9 / 2.9 / 3.6 / 4.1 | 6.4 | 7.6 | with `cuda_alloc_conf` and `opt-channelslast`

注意事项：
- batch-size大小为1时的性能约为峰值性能的**~70%**
- 峰值性能通常在batch-size为8时达到
  在此之后，如果您有额外的VRAM，它会增长几个百分点，然后由于GC开启开始下降
- 使用`lowvram`预设在批量大小低于8时性能非常低，而且到那时内存节省也不大

其他可能的优化：
- 在`webui-user.bat`中添加`set PYTORCH_CUDA_ALLOC_CONF=garbage_collection_threshold:0.9,max_split_size_mb:512`
  没有性能影响，初始内存占用略有增加，但减少了长时间运行中的内存碎片
- `opt-channelslast`
  不确定的：似乎在较高batch-size下有额外的轻微性能提升，在小batch-size下更慢，但差异在误差范围内

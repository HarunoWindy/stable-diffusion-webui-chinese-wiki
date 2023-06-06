有时将模型移动到另一个位置可能很有用。 原因可能是：
- 主磁盘磁盘空间不足
- 您在多个工具中使用模型，并且不想将它们存储两次

The default model folder is `stable-diffusion-webui/models`

## macOS Finder
- 在 Finder 中打开两个窗口，例如 `stable-diffusion-webui/models/Stable-diffusion` 和模型所在的文件夹
- 按 <kbd>option ⌥</kbd> + <kbd>command ⌘</kbd> 将模型从模型文件夹拖动到目标文件夹
- 这将创建一个别名而不是移动模型

## Command line
- 假设您的模型 `openjourney-v4.ckpt` 存储在 `~/ai/models/`
- 现在我们为这个模型建立一个symbolic link(i.e. alias)
- 打开您的terminal并导航到您的Stable Diffusion模型文件夹，e.g. `cd ~/stable-diffusion-webui/models/Stable-diffusion`
- 使用 `ln -sf ~/ai/models/openjourney-v4.ckpt` 建立一个指向你的模型的symbolic link
# What is Textual Inversion?
文本反演允许您在自己的图片上训练神经网络的一小部分，并在生成新图片时使用结果。在这种情况下，嵌入(embedding)是您训练的神经网络微小部分的名称。

训练的结果是一个.pt或.bin文件（前者是原始作者使用的格式，后者是diffusers库使用的格式），其中包含嵌入。

有关文本反演的更多详细信息，请参见原始网站：https://textual-inversion.github.io/。

# Using pre-trained embeddings
将嵌入放入`embeddings`目录中，并在提示中使用其文件名。您不必重新启动程序即可使其工作。

例如，这是我在WD1.2模型(model)上，对53张图片（119张增强）进行19500步训练，每个令牌设置8个向量得到的[Usada Pekora](https://drive.google.com/file/d/1MDSmzSbzkIcw5_aw_i79xfO3CRWQDl-8/view?usp=sharing)嵌入。

它生成的图片：
![grid-0037](https://user-images.githubusercontent.com/20920490/193285043-5d5d57d8-7b5e-4803-a211-5ca5220c35f4.png)
```
portrait of usada pekora
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 45dee52b
```

您可以在一个提示中组合多个嵌入：
![grid-0038](https://user-images.githubusercontent.com/20920490/193285265-a5224378-4ae2-48bf-ad7d-e79a9f998f9c.png)
```
portrait of usada pekora, mignon
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 45dee52b
```

在使用嵌入时要非常小心选择模型：它们在训练时使用的模型上效果很好，在不同的模型上效果不佳。例如，这是上面的嵌入和vanilla 1.4 stable diffusion模型(model)：
![grid-0036](https://user-images.githubusercontent.com/20920490/193285611-486373f2-35d0-437c-895a-71454564a7c4.png)
```
portrait of usada pekora
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 7460a6fa
```

# Training embeddings
## Textual inversion tab
用户界面中对训练嵌入的实验性支持。
- 创建一个新的空嵌入，选择带有图片的目录，在其上训练嵌入
- 该功能非常原始，使用风险自负
- 经过几万步的训练，我能够复现我在其他仓库中获得的结果，将动漫艺术家作为风格进行训练
- 支持半精度浮点数，但需要实验才能确定结果是否同样好
- 如果您有足够的内存，最好使用`--no-half --precision full`运行
- UI部分用于自动运行图像预处理。
- 您可以中断并恢复训练，而不会丢失任何数据（除了AdamW优化参数，但似乎现有的仓库都没有保存这些参数，因此普遍认为它们并不重要）
- 不支持批量大小或梯度累积
- 应该不可能使用`--lowvram`和`--medvram`标志运行此程序。

## Explanation for parameters

### Creating an embedding
- **名称**（Name）：创建嵌入的文件名。在提示（prompts）中引用嵌入时，您也将使用此文本。
- **初始化文本**（Initialization text）：您创建的嵌入最初将填充此文本的向量。如果您创建了一个名为“zzzz1234”的单向量嵌入，其初始化文本为“tree”，并且在未经训练的情况下在提示中使用它，则提示“a zzzz1234 by monet”将产生与“a tree by monet”相同的图片。
- **每个令牌的向量数**（Number of vectors per token）：嵌入的大小。此值越大，您可以将更多有关主题的信息放入嵌入中，但也会占用更多提示容许值中的单词。使用Stable Diffusion，您在提示中有75个令牌的限制。如果您在提示中使用了16个向量的嵌入，则将为您留下75-16=59的空间。另外根据我的经验，向量数越大，您需要更多图片才能获得良好结果。

### Preprocess
这将从一个目录中获取图像，处理它们以准备文本反演，并将结果写入另一个目录。这是一个方便的功能，如果您愿意，可以自己预处理图片。
- **源目录**（Source directory）：带有图像的目录
- **目标目录**（Destination directory）：将结果写入的目录
- **创建翻转副本**（Create flipped copies）：对于每个图像，也写入其镜像副本
- **将过大的图像分成两部分**（Split oversized images into two）：如果图像太高或太宽，则将其调整大小以使短边与所需分辨率匹配，并从中创建两个可能相交的图片。
- **使用BLIP标题作为文件名**（Use BLIP caption as filename）：使用询问者中的BLIP模型（model）将标题添加到文件名中。

### Training an embedding
- **嵌入**（Embedding）：从此下拉列表中选择要训练的嵌入。
- **学习率**（Learning rate）：训练应该进行得多快。将此参数设置为较高值的危险在于，如果您将其设置得过高，您可能会破坏嵌入。如果您在训练信息文本框中看到`Loss: nan`，则意味着您失败了，嵌入已死。使用默认值，这种情况不应该发生。可以使用以下语法在此设置中指定多个学习率：`0.005:100, 1e-3:1000, 1e-5` - 这将在前100步中以`0.005`的lr进行训练，然后在1000步之前以`1e-3`进行训练，然后直到结束以`1e-5`进行训练。
- **数据集目录**（Dataset directory）：用于训练的图像目录。它们都必须是正方形。
- **日志目录**（Log directory）：样本图像和部分训练过的嵌入副本将写入此目录。
- **提示模板文件**（Prompt template file）：用于训练模型（model）的提示文本文件，每行一个。请参阅目录`textual_inversion_templates`中的文件，了解您可以使用这些文件做什么。训练样式时使用`style.txt`，训练对象嵌入时使用`subject.txt`。文件中可以使用以下标签：
 - `[name]`：嵌入的名称
 - `[filewords]`：来自数据集中图像文件名的单词。有关更多信息，请参见下文。
- **最大步数**（Max steps）：完成这么多步骤后，训练将停止。一步是当一张图片（或一批图片，但当前不支持批处理）显示给模型并用于改进嵌入时。如果您中断训练并在以后恢复它，则步数保留。
- **将图像与嵌入保存在PNG块中**（Save images with embedding in PNG chunks）：每次生成图像时，它都与最近记录的嵌入组合并保存到image_embeddings中，可以作为图像共享，并放置到您的嵌入文件夹中并加载。
- **预览提示**（Preview prompt）：如果不为空，则将使用此提示生成预览图片。如果为空，则将使用来自培训的提示。

### filewords
`[filewords]`是提示模板文件的标签，允许您将文件名中的文本插入提示中。默认情况下，文件的扩展名将被删除，以及文件名开头的所有数字和破折号（`-`）。因此，这个文件名：`000001-1-a man in suit.png`将变成提示的这个文本：`a man in suit`。文件名中的文本格式保持不变。

可以使用选项`Filename word regex`和`Filename join string`来更改来自文件名的文本：例如，使用word regex = `\w+`和join string = `, `，上面的文件将产生这个文本：`a, man, in, suit`。regex用于从文本中提取单词（它们是`['a', 'man', 'in', 'suit', ]`），并在这些单词之间放置连接字符串（', '）以创建一个文本：`a, man, in, suit`。

也可以创建一个与图像具有相同文件名的文本文件（`000001-1-a man in suit.txt`），并在其中放置提示文本。将不使用文件名和regex选项。

## Third party repos
我成功地使用这些存储库训练了嵌入：

 - [nicolai256](https://github.com/nicolai256/Stable-textual-inversion_win)
 - [lstein](https://github.com/invoke-ai/InvokeAI)

其他选择是在colabs上训练和/或使用diffusers库，我对此一无所知。

# Finding embeddings online

- Github已经要求我删除此处的所有链接。

# Hypernetworks

Hypernetworks是一种新颖的（明白了吗？）概念，用于微调模型而不接触任何权重。

目前训练hypernets的方法是在文本反演选项卡中。

训练与文本反演的方式相同。

唯一的要求是使用非常非常低的学习率，例如0.000005或0.0000005。

### Dum Dum Guide
一位匿名用户编写了一份带有图片的使用hypernetworks的指南：https://rentry.org/hypernetwork4dumdums

### Unload VAE and CLIP from VRAM when training
设置选项卡上的此选项允许您以更慢的预览图片生成速度为代价节省一些内存。
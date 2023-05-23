# What is Textual Inversion?
文本反演(Textual Inversion)允许您在自己的图片上使用关键字(concept)训练神经网络。
* 关键字可以是：姿势(pose)、艺术风格、纹理等.
   * 这个关键字不必实际存在于现实世界中。例如，您可能已经看到过许多生成的图像，其否定negative prompt(np) 包含tag: "EasyNegative"。
   这是一个人工关键字，是在一质量较差的人为构思的图像上训练的。
* 它**不会丰富模型**。如果你的基础模型只在有_apples_的图像上训练, 并且你试图用大约 20 张香蕉图像来教模型_“香蕉”这个词。当你要一个香蕉时，那么你的模型会给你长长的、黄色的苹果。(当然，你可以用1000+张图像训练模型来区别香蕉和苹果，但这真的值得吗？)

训练的结果是一个.pt或.bin文件（前者是原始作者使用的格式，后者是![diffusers库](https://huggingface.co/docs/diffusers/index)使用的格式），其中包含embedding。这些内容开源。

# Using pre-trained embeddings
将嵌入放入`embeddings`目录中，并在提示中使用其文件名。您不必重新启动程序即可使其工作。

例如，这是我在WD1.2模型(model)上，对53张图片（119张增强）进行19500步训练，每个token设置8个向量得到的[Usada Pekora](https://drive.google.com/file/d/1MDSmzSbzkIcw5_aw_i79xfO3CRWQDl-8/view?usp=sharing)embedding。

它生成的图片：
![grid-0037](https://user-images.githubusercontent.com/20920490/193285043-5d5d57d8-7b5e-4803-a211-5ca5220c35f4.png)
```
portrait of usada pekora
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 45dee52b
```

您可以在一个提示中组合多个embeddings：
![grid-0038](https://user-images.githubusercontent.com/20920490/193285265-a5224378-4ae2-48bf-ad7d-e79a9f998f9c.png)
```
portrait of usada pekora, mignon
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 45dee52b
```

在使用embeddings时要非常小心选择模型：它们在当初训练时使用的模型上效果很好，在不同的模型上效果不佳。例如，这是上面的embedding和stable diffusion模型vanilla 1.4搭配的结果：
![grid-0036](https://user-images.githubusercontent.com/20920490/193285611-486373f2-35d0-437c-895a-71454564a7c4.png)
```
portrait of usada pekora
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 4077357776, Size: 512x512, Model hash: 7460a6fa
```

# Training embeddings
## Textual inversion tab
测试(Experimental)中功能：用户界面中training embeddings。
- 创建一个新的empty embedding，选择带有图片的目录上训练
- 该功能非常原始，使用风险自负
- 经过几万步的训练，我能够复现我在其他仓库中获得的结果，进行动漫风训练
- 支持半精度浮点数，但需要测试才能确定结果如何
- 如果您有足够的内存，最好使用`--no-half --precision full`运行
- UI部分用于自动运行图像预处理。
- 您可以中断或恢复训练，而不会丢失任何数据（除了AdamW优化参数，但似乎现有的仓库都没有保存这些参数，因此普遍认为它们并不重要）
- 不支持批量大小或梯度累积
- 不能使用与`--lowvram`和`--medvram`同时使用。

## Explanation for parameters

### Creating an embedding
- **Name**：创建embedding的文件名。您也将使用此name在提示（prompts）中引入embedding，。
- **Initialization text**：您创建的embedding最初将根据此文本的向量进行初始化。如果您创建了一个名为“zzzz1234”的单向量嵌入，文本为“tree”进行初始化，并且在未经训练的情况下在prompt中使用它，则prompt“a zzzz1234 by monet”产生的图片将与“a tree by monet”相同。
- **Number of vectors per token**：embedding的大小。此值越大，您可以将更多有关主题的信息放入embedding中，但也会占用更多prompt的容量。Stable Diffusion的prompt中有75个token的限制。如果您在prompt中使用了16个向量的embedding，则将为您留下75-16=59的空间。另外，向量数越大，您需要更多图片才能获得良好结果。

### Preprocess
从一个目录中获取图像，处理它们以用做textual inversion，并将结果写入另一个目录。这是一个方便的功能，可以根据自己的意愿预训练图片。
- **源目录**（Source directory）：带有图像的目录
- **目标目录**（Destination directory）：将结果写入的目录
- **创建翻转副本**（Create flipped copies）：对于每个图像，也写入其镜像副本
- **将过大的图像分成两部分**（Split oversized images into two）：如果图像太高或太宽，则将其调整大小以使短边与所需分辨率匹配，并从中创建两个可能相交的图片。
- **使用BLIP标题作为文件名**（Use BLIP caption as filename）：使用interrogator中的BLIP模型（model）将标题添加到文件名中。

### Training an embedding
- **Embedding**：从此下拉列表中选择要训练的嵌入。
- **学习率**（Learning rate）：训练应该进行得多快。将此参数设置为较高值可能会破坏embedding。如果您在训练信息文本框中看到`Loss: nan`，则意味着您失败了，embedding被破坏。使用默认值，这种情况不应该发生。可以使用以下语法在此设置中指定多个学习率：`0.005:100, 1e-3:1000, 1e-5` - 这将在前100步中以`0.005`的lr进行训练，然后在1000步之前以`1e-3`进行训练，然后直到结束以`1e-5`进行训练。
- **数据集目录**（Dataset directory）：用于训练的图像目录。它们都必须是正方形。
- **日志目录**（Log directory）：样本图像和训练未完成的embedding副本将写入此目录。
- **提示模板文件**（Prompt template file）：文本文件，每行一个的prompt, 用于训练模型。`textual_inversion_templates`目录介绍了这些文件的功能。训练style时使用`style.txt`，训练对象embedding时使用`subject.txt`。文件中可以使用以下tags：
 - `[name]`: embedding name
 - `[filewords]`：数据集中图像文件名。更多信息见下文。
- **最大步数**（Max steps）：完成这么多步骤后，训练将停止。一步是当一张图片（或一批图片，但当前不支持批处理）显示给模型并用于改进嵌入时。如果您中断训练并在以后恢复它，则步数保留。
- **将图像与嵌入保存在PNG块中**（Save images with embedding in PNG chunks）：每次生成图像时，它都与最近记录的嵌入组合并保存到image_embeddings中，可以作为图像共享，并放置到您的嵌入文件夹中并加载。
- **预览提示**（Preview prompt）：如果不为空，则将使用此提示生成预览图片。如果为空，则将使用来自培训的提示。

### filewords
`[filewords]`是prompt模板的标签，允许您将文件名中的文本插入prompt中。默认会删除文件的扩展名，文件名开头的所有数字和破折号（`-`）。因此，文件名：`000001-1-a man in suit.png`将变成prompt：`a man in suit`。文件名中的文本格式保持不变。

可以使用选项`Filename word regex`和`Filename join string`来更改来自文件名的文本：例如，使用正则表达式(regex)`\w+`和拼接字符串`, `，上面的文件将产生这个文本：`a, man, in, suit`。正则表达式用于从文本中提取单词（它们是`['a', 'man', 'in', 'suit', ]`），并在这些单词之间放置连接字符串`, `以创建一个文本：`a, man, in, suit`。

也可以创建一个与图像具有相同文件名的文本文件（`000001-1-a man in suit.txt`），并在其中放置提示文本。就可以不使用filename和正则表达式选项。

## Third party repos
我成功地使用这些存储库训练了embeddings：

 - [nicolai256](https://github.com/nicolai256/Stable-textual-inversion_win)
 - [lstein](https://github.com/invoke-ai/InvokeAI)

其他选择是在colabs上训练和/或使用diffusers库。

# Finding embeddings online

- Github已经要求我删除此处的所有链接。

# Hypernetworks

Hypernetworks是一种新概念，用于不改weights微调模型。

目前训练hypernets的方法是在textual inversion选项卡中。

训练与textual inversion的方式相同。

唯一的要求是使用非常非常低的学习率，例如0.000005或0.0000005。

### Dum Dum Guide
一位匿名用户编写了一份带有图片的使用hypernetworks的指南：https://rentry.org/hypernetwork4dumdums

### Unload VAE and CLIP from VRAM when training
设置选项卡上的此选项允许您以更慢的预览图片生成速度为代价节省一些内存。
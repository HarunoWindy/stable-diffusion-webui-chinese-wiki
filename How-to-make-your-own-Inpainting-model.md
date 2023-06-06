Making your own inpainting model is very simple:


![screenshot](https://github.com/AUTOMATIC1111/stable-diffusion-webui/assets/40751091/4bdbab38-9237-48ea-9698-a036a5c96585)

1. Go to Checkpoint Merger
2. 选择 "Add Difference"
3. Set "Multiplier" to 1.0
4. Set "A" to the official inpaint model ([SD-v1.5-Inpainting](https://huggingface.co/runwayml/stable-diffusion-inpainting/tree/main))
5. Set "B" to your model
6. Set "C" to the standard base model ([SD-v1.5](https://huggingface.co/runwayml/stable-diffusion-v1-5/tree/main))
7. 将名称设置为您想要的任何名称，可能是: (your model)_inpainting
8. 将其他值设置为首选，i.e. 可能选择 "Safetensors" and "Save as float16"
9. 点击合并
10. 在img2img绘图tab中使用您的新模型

它的工作方式实际上只是简单照搬inpainting model，然后将你的模型的独特数据拷贝给它。


请注意， 公式是`A + (B - C)`, 您可以将其解释为等同于`(A - C) + B`.

因为 `A = 1.5-inpaint`, `C = 1.5`

所以`A - C = inpainting逻辑`

所以公式是`(Inpainting 逻辑) + (Your Model)`.

### Wider Application

This "add-the-difference" merging can be applied to almost all the **mechanically unique** models webui can load. 
这种"add-the-difference"合并可以应用于几乎所有 webui 可以加载的 **mechanically unique** 模型。
 See [Features](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features)


#1 您现有的 **finetuned model** 需要匹配 **unique model's** 架构, 或者: Stable Diffusion 2, or 1. 

#2 您还需要将 unique model 与 base model 进行对比。 
Find out what was the base model from their github.

Q: altdiffusion-m9 使用什么作为基础模型？ \
A: stable diffusion 1.4 model

Q: instructpix2pix 使用什么作为基础模型? \
A: stable diffusion 1.5 model

这些模型的 networks/properties 微调即可使用，就像著名的 controlnet networks的应用一样，只是这些与模型没有分离。

<details><summary> Notes: </summary>

_您可能意识到 Controlnet networks 网络已经可以做很多这样的事情了_ 

所以，这里有一些可能值得尝试的事情:

-更暗/更亮 的光线 with noise offset model \
-使用 miniSD 模型以较小的256 or 320尺寸制作与 512x512 相似的图片 \
-prompt 更有确定性 across input languages with altdiffusion-m9 model (changes clip model)

</details>

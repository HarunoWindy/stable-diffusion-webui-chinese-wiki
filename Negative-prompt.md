负面提示(negative prompt)是一种用户指定不想看到的内容方式，它不需要为模型(model)增加额外的负担或要求。据我所知，我首次使用的commmit是[757bb7c4](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/757bb7c46b20651853ee23e3109ac4f9fb06a061)。这个功能常用来消除Stable Diffusion的常见变形（如额外的四肢）或不想看到的内容(有时也可以通过prompt实现)。不消耗prompt的75个token上限。

负面提示(negative prompt)的工作方式是在进行采样时，`unconditional_conditioning`设置为指定的文本代替是字符串。

下面是来自[txt2img.py](https://github.com/CompVis/stable-diffusion/blob/main/scripts/txt2img.py)的（简化版）代码：

```python
# prompts = ["a castle in a forest"]
# batch_size = 1

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(batch_size * [""])

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

启动采样器，重复执行以下操作：
- 降噪(de-noises)图片，使其看起来更像你的提示(prompt)（conditioning）
- 降噪(de-noises)图片，使其看起来更像空提示(unconditional_conditioning)
- 查看两者之间的差异，并使用它根据噪声图片生成一组更改（不同采样器执行不同的部分）

要使用负面提示(negative prompt)，只需要这样做：

```python
# prompts = ["a castle in a forest"]
# negative_prompts = ["grainy, fog"]

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(negative_prompts)

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

然后采样器将观察降噪(de-noised)后看起来像prompt(城堡)与看起来像negative prompt(颗粒状、雾)的图像之间的差异，并尝试将最终结果向P移动并远离NP。

### Examples:

```
a colorful photo of a castle in the middle of a forest with trees and (((bushes))), by Ismail Inceoglu, ((((shadows)))), ((((high contrast)))), dynamic shading, ((hdr)), detailed vegetation, digital painting, digital drawing, detailed painting, a detailed digital painting, gothic art, featured on deviantart
Steps: 20, Sampler: Euler a, CFG scale: 7, Seed: 749109862, Size: 896x448, Model hash: 7460a6fa
```

| negative prompt     | image                                                                                                                     |
|---------------------|---------------------------------------------------------------------------------------------------------------------------|
| none                | ![01069-749109862](https://user-images.githubusercontent.com/20920490/192156368-18360487-0dcf-4b7d-b57e-b3fa80a81f1a.png) |
| fog                 | ![01070-749109862](https://user-images.githubusercontent.com/20920490/192156405-9c43ba8c-4eb8-415d-9f4d-902c8cf69b6d.png) |
| grainy              | ![01071-749109862](https://user-images.githubusercontent.com/20920490/192156421-17e53296-df5c-4e82-bf9a-f1ca562d3ad0.png) |
| fog, grainy         | ![01072-749109862](https://user-images.githubusercontent.com/20920490/192156430-3d05e5c4-2b86-409c-a357-a31178e0cb30.png) |
| fog, grainy, purple | ![01073-749109862](https://user-images.githubusercontent.com/20920490/192156440-ec59abe8-1a18-4372-a100-0da8bc1f8d13.png) |

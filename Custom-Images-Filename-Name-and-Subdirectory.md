> 以下信息是关于图像文件名和子目录名, not the `Paths for saving \ Output directorys`
### 默认情况下，Web UI 将图像保存在输出目录中，文件名结构为

`number`-`seed`-`[prompt_spaces]`

```
01234-987654321-((masterpiece)), ((best quality)), ((illustration)), extremely detailed,style girl.png
```

如果用户希望，可以使用不同的图像文件名和可选的子目录。

可以在以下位置配置图像文件名模式。

`settings tab` > `Saving images/grids` > `Images filename pattern`

子目录可以在以下位置配置.

`settings tab` > `Saving to a directory` > `Directory name pattern`

# Patterns
Web-UI提供了几种模式，可以用作将信息插入文件名或子目录的占位符。用户可以将这些模式链接在一起，形成适合他们用例的文件名。

| Pattern                        | Description                                          | Example                                                                                                                               |
|--------------------------------|------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `[seed]`                       | Seed                                                 | 1234567890                                                                                                                            |
| `[steps]`                      | Steps                                                | 20                                                                                                                                    |
| `[cfg]`                        | CFG scale                                            | 7                                                                                                                                     |
| `[sampler]`                    | Sampling method                                      | Euler a                                                                                                                               |
| `[model_name]`                 | name of the model                                    | sd-v1-4
| `[model_hash]`                 | Hash of the model                                    | 7460a6fa                                                                                                                              |
| `[width]`                      | Image width                                          | 512                                                                                                                                   |
| `[height]`                     | Image hight                                          | 512                                                                                                                                   |
| `[styles]`                     | Name of the chosen Styles                            | my style name                                                                                                                         |
| `[date]`                       | Date of the computer in ISO format                   | 2022-10-24                                                                                                                            |
| `[datetime]`                   | Datetime in "%Y%m%d%H%M%S"                           | 20221025013106                                                                                                                        |
| `[datetime<Format>]`           | Datetime in specified \<Format\>                       | \[datetime<%Y%m%d_%H%M%S_%f>]<br>20221025_014350_733877                                                                                   |
| `[datetime<Format><TimeZone>]` | Datetime at specific \<Time Zone\> in specified \<Format\> | \[datetime<%Y%m%d_%H%M%S_%f><Asia/Tokyo>]`<br>20221025_014350_733877                                                                                       |
| `[prompt_no_styles]`           | Prompt without Styles                                | 1gir,   white space, ((very   important)), [not important], (some value_1.5), (whatever), the end<br>                                     |
| `[prompt_spaces]`              | Prompt with Styles                                   | 1gir,   white space, ((very   important)), [not important], (some value_1.5), (whatever), the end<br>,   (((crystals texture Hair)))，((( |
| `[prompt]`                     | Prompt with Styles, `Space bar` replaced with`_`       | 1gir,\_\_\_white_space,\_((very\_important)),\_[not\_important],\_(some\_value\_1.5),\_(whatever),\_the\_end,\_(((crystals_texture_Hair)))，(((     |
| `[prompt_words]`               | Prompt   with Styles, Bracket(括号) and Comma(逗号) removed      | 1gir white space very important not important some value 1 5 whatever the   end crystals texture Hair ， extremely detailed           |
| `[prompt_hash]` | The first 8 characters of the prompt's SHA-256 hash | 1girl -> 6362d0d2<br>(1girl:1.1) -> 0102e068 |
| `[clip_skip]` | CLIP stop at last layers(图层) | 1 |
| `[batch_number]` | 单个批处理作业中的第N张图像 | BatchNo_[batch_number] -> BatchNo_3
| `[generation_number]` | 整个作业中的第N张图像 | GenNo_[generation_number] -> GenNo_9
| `[hasprompt<prompt1\|default><prompt2>...]` | 如果在提示（prompts）中找到指定的“prompt”，则将“prompt”添加到文件名中；否则，将“default”添加到文件名中（“default”可以为空） | [hasprompt<girl><boy>] -> girl<br>[hasprompt<girl\|no girl><boy\|no boy>] -> girlno boy

### Datetime Formatting details
更多详细信息，请参考 python 文档 [Format Codes](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)

### Datetime Time Zone details
参考 [List of Time Zones](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/List-of-Time-Zones) 获取有效时区的列表。

如果`<Format>`为空或无效，将使用默认的时间格式"%Y%m%d%H%M%S"。
提示：您可以在`<Format>`中使用额外的字符作为标点符号，例如`_ -`。

如果`<TimeZone>`为空或无效，将使用默认的系统时区。

如果`batch size`为1，则不会将`[batch_number]`以及前面的文本段添加到文件名中。

如果`batch size` x `batch count`为1，则不会将`[generation_number]`以及前面的文本段添加到文件名中。

上述`[prompt]`示例中使用的提示和样式。

Prompt:
```
1girl,   white space, ((very important)), [not important], (some value:1.5), (whatever), the end
```
Selected Styles:
```
(((crystals texture Hair)))，(((((extremely detailed CG))))),((8k_wallpaper))
```

note: 上面提到的`Styles`是指generate按钮下面的两个下拉菜单。

### if the Prompts is too long, it will be cutoff
这是因为您的计算机有最大文件长度

# Add / Remove number to filename when saving
您可以通过取消选中下面的复选框删除前缀号码

`Setting` > `Saving images/grids` > `Add number to filename when saving`

with prefix number
```
00123-`987654321-((masterpiece)).png
```

without prefix number
```
987654321-((masterpiece)).png
```

### Caution
前缀数字的目的是确保保存的图像文件名是**唯一**的。
如果您决定不使用前缀数字，请确保您的模式能够生成唯一的文件名。

**Otherwise files might be Overwritten**

一般datetime精确到秒应该可以保证文件名是唯一的。

```
[datetime<%Y%m%d_%H%M%S>]-[seed]
``` 
```
20221025_014350-281391998.png
```

But some **Custom Scripts** might generate **multiples images** using the **same seed** in a **single batch**,

in this case it is safer to also use `%f` for `Microsecond as a decimal number, zero-padded to 6 digits.`

但是一些**自定义脚本**可能会在**单个批次**中使用**相同的种子(seed)**生成**多个图像**，

在这种情况下，更安全的做法是将“微秒”的“%f”用作十进制数，用零填充到 6 位数字。

```
[datetime<%Y%m%d_%H%M%S_%f>]-[seed]
```
```
20221025_014350_733877-281391998.png
```

# Filename Pattern Examples

如果您在多台机器上运行Web-Ui，例如在Google Colab和您自己的计算机上，您可能希望使用带有时间作为前缀的文件名。
这样，当您下载这些文件时，您可以将它们放在同一个文件夹中。

此外，由于您不知道Google Colab使用的时区，您可能希望指定时区。
```
[datetime<%Y%m%d_%H%M%S_%f><Asia/Tokyo>]-[seed]-[prompt_words]
```
```
20221025_032649_058536-3822510847-1girl.png
```

设置子目录的日期可能也很有用，这样一个文件夹就不会有太多图像
```
[datetime<%Y-%m-%d><Asia/Tokyo>]
```
```
2022-10-25
```

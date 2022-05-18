---
title: LabelMe2seg无法生成真值图片的问题解决
tags:
  - DeepLearning
  - bugs
abbrlink: e9d03737
date: 2022-04-23 16:47:03
---

最近在LabelMe标注好图片之后，使用labelme2seg无法生成真值图片，报错如下：

```shell
Generating dataset from: roadscene_train\img001.json
Traceback (most recent call last):
  File "labelme2voc.py", line 95, in <module>
    main()
  File "labelme2voc.py", line 85, in main
    labelme.utils.lblsave(out_png_file, lbl)
  File "...\.conda\envs\label_me\lib\site-packages\labelme\utils\_io.py", line 14, in lblsave
    if lbl.min() >= -1 and lbl.max() < 255:
AttributeError: 'tuple' object has no attribute 'min'
```

在网上搜寻解决方案未果，遂尝试自行排查，最后发现问题出现在`labelme2seg.py`本身，是labelme更新之后的接口冲突问题，只需修改

```python
 lbl= labelme.utils.shapes_to_label(
```

这一行如下：

```python
lbl, _ = labelme.utils.shapes_to_label(
```

即可解决问题

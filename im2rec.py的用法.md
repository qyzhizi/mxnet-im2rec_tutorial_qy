在使用`im2rec.py`之前需要编写一个脚本文件，用于生成所需的list文件．
`im2rec.py` 会使用python3 cv2
,因此要安装cv2(opencv的python接口)下面的是用法，默认生成的rec文件中图片是打乱的

```{.python .input .}
usage: im2rec.py [-h] [--list] [--exts EXTS [EXTS ...]] [--chunks CHUNKS]
                 [--train-ratio TRAIN_RATIO] [--test-ratio TEST_RATIO]
                 [--recursive] [--no-shuffle] [--pass-through]
                 [--resize RESIZE] [--center-crop] [--quality QUALITY]
                 [--num-thread NUM_THREAD] [--color {-1,0,1}]
                 [--encoding {.jpg,.png}] [--pack-label]
                 prefix root
```

在终端使用命令：
```
$ ~/software/anaconda3/envs/mxnet/bin/python im2rec.py  --pack-
label dataset ./pic_select
````
上面的`dataset`是`prefix`,运行上面命令时，list文件应该是`dataset.list`,最后生成的`rec`文件也是这个`prefix`,`root`是指生成list文件时同时挑选出来的图片，`--pack-
label`是指将标签和rec文件一起打包<br>
`~/software/anaconda3/envs/mxnet/bin/python`是指python的路径，`im2rec.py`
使用python3，这里是使用anaconda3的mxnet环境(配置的）中的python

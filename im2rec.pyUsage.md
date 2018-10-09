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

```{.python .input}
$ ~/software/anaconda3/envs/mxnet/bin/python im2rec.py  --pack-
label dataset ./pic_select
````
上面的`dataset`是`prefix`,运行上面命令时，list文件应该是`dataset.list`,最后生成的`rec`文件也是这个`prefix`,`root`是指生成list文件时同时挑选出来的图片，`--pack-
label`是指将标签和rec文件一起打包<br>
`~/software/anaconda3/envs/mxnet/bin/python`是指python的路径，`im2rec.py`
使用python3，这里是使用anaconda3的mxnet环境(配置的）中的python

```{.python .input}
# 将
~/software/anaconda3/envs/gluon/bin/python im2rec.py  --pack-label ownSet /media/qy/My\ Passport/coco_dataset/ownSet
```

### 运行情况

```{.python .input}
~/software/anaconda3/envs/gluon/bin/python im2rec.py  --pack-label ownSet /media/qy/My\ Passport/coco_dataset/ownSet
```

```{.python .input}
'''
qy@QY [14时44分55秒] [~/software/mxnet-im2rec_tutorial_qy] [master *]
-> % ~/software/anaconda3/envs/gluon/bin/python im2rec.py  --pack-label ownSet
/media/qy/My\ Passport/coco_dataset/ownSet                            
Creating
.rec file from /home/qy/software/mxnet-im2rec_tutorial_qy/ownSet.lst in
/home/qy/software/mxnet-im2rec_tutorial_qy
multiprocessing not available, fall back to single threaded encodingq
time: 0.5816903114318848  count: 0
time: 25.495652675628662  count: 1000
time: 29.71488070487976  count: 2000
time: 28.914949417114258  count: 3000
time: 27.63347887992859  count: 4000
time: 32.3925302028656  count: 5000
time: 23.157869338989258  count: 6000
time: 35.49057459831238  count: 7000
time: 33.37408447265625  count: 8000
time: 32.5582070350647  count: 9000
time: 29.552295207977295  count: 10000
time: 26.418681621551514  count: 11000
time: 25.729156494140625  count: 12000
time: 27.79803991317749  count: 13000
time: 28.886123418807983  count: 14000
time: 23.227732181549072  count: 15000
time: 21.852363109588623  count: 16000
time: 26.696550607681274  count: 17000
time: 20.59850788116455  count: 18000
time: 20.907175302505493  count: 19000
time: 22.386571645736694  count: 20000
'''
```

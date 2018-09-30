## 从coco的数据集中提取某一类别的图片和标签

### 下载coco数据集
http://cocodataset.org/#download<br>
笔者下载2017年的数据集，下载的本地路径：`/media/qy/My Passport/coco_dataset/2017_dataset`<br>
`2017_dataset`包含`annotations` `train2017` `val2017`
三个文件夹，`annotations`是各种标签文件，其中就包含了各类物体的边框标签文件--`instances_val2017.json`，`train2017`是训练图片集，`val2017`是验证图片集<br>
### 相关的API
#### coco的API demo
https://github.com/cocodataset/cocoapi/blob/master/PythonAPI/pycocoDemo.ipynb
#### coco API
https://github.com/cocodataset/cocoapi

### coco数据集的Object
Detection数据结构
http://cocodataset.org/#format-data

{
"info" : info, 
"images" : [image],
#[image]是一个列表，列表中的每一个元素是一个字典，字典结构如下面image{}所示
"annotations" : [annotation], #同上
"licenses" : [license],#同上
"categories" : [categories],#同上
}

info{
"year" :
int, 
"version" : str, 
"description" : str, 
"contributor" : str, 
"url" : str,
"date_created" : datetime,
}

image{
"id" : int,
"width" : int,
"height" : int,
"file_name" : str, 
"license" : int, 
"flickr_url" : str, 
"coco_url" : str,
"date_captured" : datetime,
}

license{
"id" : int,
"name" : str, 
"url" : str,
}

annotation{
"id" : int, 
"image_id" : int, 
"category_id" : int,
"segmentation" : RLE or [polygon], 
"area" : float, 
"bbox" :
[x,y,width,height], 
"iscrowd" : 0 or 1,
}

categories{
"supercategory" :str,
"id" : int,
"name" : str,
}

```{.python .input}
import json  
from mxnet import image  
from skimage import io  
import os  
  
## load COCO annotations  
with open('/media/qy/My Passport/coco_dataset/2017_dataset/annotations/instances_train2017.json', 'r') as f:  
    DataSets = json.load(f)  
print(DataSets['annotations'][0]) 
```

```{.python .input}
for key in DataSets.keys():#说明 DataSets 是一个字典 有5个元素
    print(key)
```

```{.python .input}
print(len(DataSets['categories']))# 类别的种类
print(len(DataSets['categories'][0]))#每个类别包含的元素的个数
print(DataSets['categories'][0])#输出第一个类别的信息
print(DataSets['categories'])#输出所有类别的信息
```

### 其他一些操作

```{.python .input}
print(len(DataSets['info']))#6 
print(len(DataSets['images']))#118287 DataSets['images']包含图片除了标注之外的信息，如网络上的地址等，一共118287张图片
print(len(DataSets['licenses']))#8 共8个licenses
print(len(DataSets['annotations']))#860001 之所以和print(len(DataSets['images']))的结果不一样，我猜是应为一张图片有多个目标导致的
```

```{.python .input}
#查看DataSets['images']的信息，第一张图片的信息
print(DataSets['images'][0])
print(len(DataSets['images'][0]))
```

```{.python .input}
#查看某一张图片的具体信息，比如000000119752.jpg的信息
for i in range(len(DataSets['images'])):
    if DataSets['images'][i]['file_name']=='000000119752.jpg':
        print(DataSets['images'][i])
        break
```

```{.python .input}
#查看DataSets['info']的信息
print(DataSets['info'])
print(len(DataSets['info']))
```

```{.python .input}
print(DataSets['licenses'][0])
print(len(DataSets['licenses'][0]))
```

### 查找某一类别的id
id:  44    类别: bottle<br>
id:  46    类别: wine glass<br>
id:  47
类别: cup<br>

```{.python .input}
#查看所有类别的id和名称
for i in range(len(DataSets['categories'])):
    print("id: ",DataSets['categories'][i]['id']," ","序号:",i+1,"类别:",DataSets['categories'][i]['name'])
```

```{.python .input}
#查看所有类别的名称
for i in range(len(DataSets['categories'])):
    print(DataSets['categories'][i]['name'])
```

### 提取某一类别的标签，生成自定义（gluon)的标签文件（xxx.json)，以及特定类别的图片集合

```{.python .input}
## save class and own dataset .json  
jsonName = 'ownset.json'  #关于杯子的json标签
directory = '/media/qy/My Passport/coco_dataset/ownSet/' #将提取的杯子图片保存在移动硬盘的路径里 
data = {}  
data['DataSet'] = []  
with open(jsonName, 'w') as outfile:  
    if not os.path.exists(directory):  
        os.makedirs(directory)  
    for DataSet in DataSets['annotations']:  
        box = DataSet['bbox']  
        default_name = "000000000000"  
        img_id = str(DataSet['image_id'])  
        img_name = default_name[:len(default_name) - len(img_id)] + str(img_id) + '.jpg'  
        coco_name = '/media/qy/My Passport/coco_dataset/2017_dataset/train2017/' + img_name  #获取图片的路径
        if DataSet['category_id'] == 47:  #cup 选择特定类别的图片
  
            with open(coco_name, 'rb') as f:  
                img = image.imdecode(f.read())  
                height = img.shape[0]  
                width  = img.shape[1]  
                box[0] = box[0]/width  #normalize
                box[2] = box[2]/width  
                box[1] = box[1]/height  
                box[3] = box[3]/height  
                io.imsave(directory + img_name, img.asnumpy()) #将这类图片存放到特定路径 
                data['DataSet'].append({  
                'img_name': img_name,  
                'height': height,  
                'width': width,  
                'bbox': box,  
                'class':DataSet['category_id']  
                })  
    json.dump(data, outfile)  
print('save ok')
```

```{.python .input}
with open(jsonName, 'r') as f:  
    Sets = json.load(f)
print(Sets['DataSet'][0])
```

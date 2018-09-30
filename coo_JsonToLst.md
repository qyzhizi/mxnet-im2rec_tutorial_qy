```{.python .input  n=1}
import json  
import mxnet as mx  
from skimage import io 
```

```{.python .input  n=2}
jsonName = 'ownset.json' #当前文件夹的某一类的标签文件
#directory = 'ownSet/'  
directory ='/media/qy/My Passport/coco_dataset/ownSet/'#提取的某一类的图片集
with open(jsonName, 'r') as f:  
    DataSet = json.load(f)  #读取标签文件
  
print(DataSet['DataSet'][0]['img_name'])  #答应第一张图片的name
```

```{.json .output n=2}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "000000368402.jpg\n"
 }
]
```

```{.python .input  n=3}
#查看第一张图片的标签
print(DataSet['DataSet'][0])  
```

```{.json .output n=3}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "{'img_name': '000000368402.jpg', 'height': 427, 'width': 640, 'bbox': [0.41042187500000005, 0.6089461358313817, 0.051859375, 0.14840749414519905], 'class': 47}\n"
 }
]
```

```{.python .input  n=16}
#查看数据的类型
print(DataSet['DataSet'][0]['bbox'])
print(type(DataSet['DataSet'][0]['height']))
print(type(DataSet['DataSet'][0]['bbox']))
print(type(int(DataSet['DataSet'][0]['height'])))#整型使用整型强制转换之后还是整型
```

```{.json .output n=16}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "[0.41042187500000005, 0.6089461358313817, 0.051859375, 0.14840749414519905]\n<class 'int'>\n<class 'list'>\n<class 'int'>\n"
 }
]
```

```{.python .input  n=4}
#查看图片的数量
j=0
for i in range(len(DataSet['DataSet'])):
    j=j+1
    k=i
print(j,k)
```

```{.json .output n=4}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "20650 20649\n"
 }
]
```

```{.python .input  n=5}
img_idx=0
#with open('ownSet.lst', 'w+') as f:
with open('./ownSet.lst', 'w+') as f:  
    for Data in DataSet['DataSet']:  

        x_min = Data['bbox'][0]  
        y_min = Data['bbox'][1]  
        x_max = Data['bbox'][0]+ Data['bbox'][2]  
        y_max = Data['bbox'][1]+ Data['bbox'][3] 
        f.write(  
                str(img_idx) + '\t' +  # idx  
                str(4) + '\t' + str(5) + '\t' +  # width of header and width of each object.  
                #str(int(Data['height'])) + '\t' + str(Data['width']) + '\t' +  # (width, height)
                str(int(Data['width'])) + '\t' + str(Data['height']) + '\t' +  # (width, height)
                str(1) + '\t' +  # class  
                str(x_min) + '\t' + str(y_min) + '\t' + str(x_max) + '\t' + str(y_max) + '\t' +  # xmin, ymin, xmax, ymax  
                str(Data['img_name'])+'\n')  
        img_idx += 1
```

```{.python .input  n=6}
print(img_idx)#查看图片的数量
```

```{.json .output n=6}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "20650\n"
 }
]
```

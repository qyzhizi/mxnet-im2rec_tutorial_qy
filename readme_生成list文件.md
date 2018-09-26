
# 生成所需的ｌｉｓｔ文件　并将图片拷贝到新文件

## 本文是为了将篮球数据集的数据格式转换为mxnet格式所需要的格式(rec格式)，需要转换原有的．xml格式的标签文件转换为需要的list文件．篮球数据集包含三部分：Annotations JPEGImages val.txt


```python
import sys
import xml.etree.ElementTree as ET
from mxnet import image  
from skimage import io  
import os 
import shutil
```


```python
#fileName='./label_id.txt'
fileName='./val.txt'#存放者训练数据集的图片的标号
fileName2='./Annotations/'#各个图片的标签.xml 存放的文件夹
#pic_sel_dir='./pic_select/'
pic_sel_dir='./pic_select_val/'
if not os.path.exists(pic_sel_dir):  
        os.makedirs(pic_sel_dir)
        
#fp=open(fileName,'r')
with open(fileName,'r') as fp:
    #lines=fp.readlines()
    i=0
    for line in fp:
        
        #fp1 = open('./test2.txt','a')
        #line=fp.readline()
        line=line.rstrip()#去除后面的换行符
        xml_dir=fileName2+line+'.xml'# 图片的标签文件的路径
        #print(xml_dir)
        
        #将图片复制到新文件夹
        oldname='./JPEGImages/'+line+'.JPEG'#需要拷贝的文件的路径和文件名
        newname=pic_sel_dir+str(i)+'.jpg'#新路径＋新文件名
        shutil.copyfile(oldname,newname)#复制图片
        
        tree = ET.ElementTree(file=xml_dir)#打开xml_dir
        p_label=tree.getroot()#获取根目录
        #print(p_label)#显示根目录
        #for _i in range(4):
            #print(p_label[5][4][_i].text)#打印bbox
        #打开.lst 文件　以下为转换的规律
        with open('./dataset_val.lst', 'a') as f:
            width=(float)(p_label[3][0].text)
            height=(float)(p_label[3][1].text)
            x_min=(float)(p_label[5][4][0].text)/width
            y_min=(float)(p_label[5][4][1].text)/height
            x_max=(float)(p_label[5][4][2].text)/width
            y_max=(float)(p_label[5][4][3].text)/height
            f.write(  
            str(i) + '\t' +  
            # idx  
            str(4) + '\t' + str(5) + '\t' +  
            # width of header and width of each object.  
            str(width) + '\t' + str(height) + '\t' +  
            # (width, height)  
            str(0) + '\t' +  
            # class  
            str(x_min) + '\t' + str(y_min) + '\t' + str(x_max) + '\t' +str(y_max) + '\t' +  
            # xmin, ymin, xmax, ymax  
           str(i) + '.jpg\n'
            )
        i=i+1#行标号
        #fp1.write(line)
        #fp1.close()
        #if(line=='n02802426_100\n'):
            #print('n02802426_100')
            #fp1 = open('./test2.txt','a')
            #fp1.write('n02802426_100\n')
            #fp1.close
#fp.close()
print(i)
```

    174


# Description
we propose a novel hardware-friendly approach utilizing YOLOv8n, specifically designed for  unmanned devices to detect drowning patients at sea.
# **Experimental** **setup**
This study's experimental configuration was established on a system operating under Windows 10, outfitted with an i5-12600KF CPU and an NVIDIA GeForce RTX 4060 GPU. Experiments were accelerated using CUDA 11.8, based on PyTorch 2.2.0, with identical hyper-parameters for  training and validation. 
# Download
The dataset  have been released. [Dataset Link]( https://pan.baidu.com/s/1B5faBpCeaCFtFzeMmPpn5w?pwd=d84c )
- These data images are publicly available data that I filtered on the roboflow website and re annotated.
- If you are interested in this project, you can obtain the dataset through the above link or download it directly from the branch master. 
### Dataset Directory Structure
~~~
dataset/
	└── train
		└── images (folder including all training images)
		└── labels (folder including all training labels)
	└── test
		└── images (folder including all testing images)
		└── labels (folder including all testing labels)
	└── valid
		└── images (folder including all testing images)
		└── labels (folder including all testing labels)
-> data/dataset.yaml
~~~
### xml_txt
~~~
import os  
import glob  
import xml.etree.ElementTree as ET  
  
xml_file=r'' #xml_file  
  
l=['bottle']  
  
def convert(box,dw,dh):  
    x=(box[0]+box[2])/2.0  
  y=(box[1]+box[3])/2.0  
  w=box[2]-box[0]  
    h=box[3]-box[1]  
  
    x=x/dw  
    y=y/dh  
    w=w/dw  
    h=h/dh  
  
    return x,y,w,h  
  
def f(name_id):  
    xml_o=open(r''%name_id)  # original xml file path
    txt_o=open(r''%name_id,'w')  # the current txt file
  
    pares=ET.parse(xml_o)  
    root=pares.getroot()  
    objects=root.findall('object')  
    size=root.find('size')  
    dw=int(size.find('width').text)  
    dh=int(size.find('height').text)  
  
    for obj in objects :  
        c=l.index(obj.find('name').text)  
        bnd=obj.find('bndbox')  
  
        b=(float(bnd.find('xmin').text),float(bnd.find('ymin').text),  
  float(bnd.find('xmax').text),float(bnd.find('ymax').text))  
  
        x,y,w,h=convert(b,dw,dh)  
  
        write_t="{} {:.5f} {:.5f} {:.5f} {:.5f}\n".format(c,x,y,w,h)  
        txt_o.write(write_t)  
  
    xml_o.close()  
    txt_o.close()  
  
name=glob.glob(os.path.join(xml_file,"*.xml"))  
for i in name :  
    name_id=os.path.basename(i)[:-4]  
    f(name_id)
~~~

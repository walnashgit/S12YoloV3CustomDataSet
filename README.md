# YoloV3
________
YoloV3 Simplified for training on Colab with custom dataset. 

_A Collage of Training images_
![image](https://github.com/walnashgit/S12YoloV3CustomDataSet/assets/73463300/4ac233ae-b67d-405c-9a26-a6113e68f3c1)



We have added a very 'smal' Coco sample imageset in the folder called smalcoco. This is to make sure you can run it without issues on Colab.

Full credit goes to [this](https://github.com/ultralytics/yolov3), and if you are looking for much more detailed explainiation and features, please refer to the original [source](https://github.com/ultralytics/yolov3). 

### School of AI Repo:
https://github.com/theschoolofai/YoloV3

### Steps to train and validate:

You'll need to download the weights from the original source. 
1. Create a folder called weights in the root (YoloV3) folder
2. Download from: https://drive.google.com/file/d/1vRDkpAiNdqHORTUImkrpD7kK_DkCcMus/view?usp=share_link
3. Place 'yolov3-spp-ultralytics.pt' file in the weights folder:
  * to save time, move the file from the above link to your GDrive
  * then drag and drop from your GDrive opened in Colab to weights folder
4. run this command
`python train.py --data data/smalcoco/smalcoco.data --batch 10 --cache --epochs 25 --nosave`

For custom dataset:
1. Clone this repo: https://github.com/miki998/YoloV3_Annotation_Tool
2. Follow the installation steps as mentioned in the repo. 
3. In the repo I have downloaded and annotated 100 images of tabla.
4. Annotate the images using the Annotation tool. 
```
data
  --customdata
    --images/
      --img001.jpg
      --img002.jpg
      --...
    --labels/
      --img001.txt
      --img002.txt
      --...
    custom.data #data file
    custom.names #your class names
    custom.txt #list of name of the images you want your network to be trained on. Currently we are using same file for test/train
```
5. As you can see above you need to create **custom.data** file. For 1 class example, your file will look like this:
```
  classes=1
  train=data/customdata/custom.txt
  test=data/customdata/custom.txt 
  names=data/customdata/custom.names
```
6. It is a poor idea to keep test and train data same, but the point of this repo is to get you up and running with YoloV3 asap. This is how our file looks like (please note the .s and /s):
```
./data/customdata/images/Tabla_01.jpg
./data/customdata/images/Tabla_02.jpg
./data/customdata/images/Tabla_03.jpg
...
```
7. You need to add custom.names file as you can see above. For our example, we downloaded images of Tabla. Our custom.names file look like this:
```
Tabla
```
8. Tabla above will have a class index of 0. 
9. For COCO's 80 classes, VOLOv3's output vector has 255 dimensions ( (4+1+80)*3). Now we have 1 class, so we would need to change it's architecture.
10. Copy the contents of 'yolov3-spp.cfg' file to a new file called 'yolov3-custom.cfg' file in the cfg folder. 
11. Search for 'filters=255' (you should get entries entries). Change 255 to 18 = (4+1+1)*3
12. Search for 'classes=80' and change all three entries to 'classes=1'
13. Since you are lazy (probably), you'll be working with very few samples. In such a case it is a good idea to change:
  * burn_in to 100
  * max_batches to 5000
  * steps to 4000,4500
14. Don't forget to perform the weight file steps mentioned in the sectio above. 
15. Run this command to train `python train.py --data data/customdata/custom.data --batch 10 --cache --cfg cfg/yolov3-custom.cfg --epochs 3 --nosave`
16. Run this command to test: `python test.py --data data/customdata/custom.data --batch 10 --cfg cfg/yolov3-custom.cfg  --weights weights/last.pt`
17. Run this command for inference: `python detect.py`. It will use the images from the custom folder and save the output in 'output' folder.

As you can see in the collage image above, a lot is going on, and if you are creating a set of say 500 images, you'd get a bonanza of images via default augmentations being performed. 


**Results**
After training for 250 Epochs, this is the result

![image](https://github.com/walnashgit/S12YoloV3CustomDataSet/assets/73463300/972326b1-d3e6-40a4-a229-2eebe1126d1e)



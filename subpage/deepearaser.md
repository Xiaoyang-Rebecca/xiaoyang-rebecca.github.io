## Deep Eraser

I designed an object-oriented "eraser" for image and video , which is able to remove the pixels with designated type. MRCNN is used to dected the pixel of a specific types then WGAN is used to remove the detect pixels as they never exist. 

<p align="center"><img src="./figures/Slide3.PNG" width="400" class="inline"/></p>
*This is just a prototype use COCO pretrained weights without any additional training.

- Demo for Video
<p align="center">
<img src="./figures/clip1_borded.gif" width="250" class="inline"/>
<img src="./figures/clip1_erased.gif" width="250" class="inline"/></p>

- [[PythonCode]](https://github.com/Xiaoyang-Rebecca/DeepEraser)

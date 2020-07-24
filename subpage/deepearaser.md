## Deep Eraser
--------------

I designed an object-oriented “eraser” for image and video, which is able to remove the pixels with designated types. MRCNN is used to detect the pixels of a specific type, and then WGAN is used to remove the detect pixels as they never exist

<p align="center"><img src="../figures/Slide3.PNG" width="400" class="inline"/></p>

- Demo for Video
<p align="center">
<img src="../figures/clip1_borded.gif" width="250" class="inline"/>
<img src="../figures/clip1_erased.gif" width="250" class="inline"/></p>

- [[PythonCode]](https://github.com/Xiaoyang-Rebecca/DeepEraser)


*This is a prototype only using COCO pretrained weights without any additional training.

---
<< [Back](../)
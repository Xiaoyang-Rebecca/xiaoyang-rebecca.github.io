---
title: "Deep Eraser"
excerpt: "An object-oriented “eraser” for images and videos <br/><img src='/figures/logo-eraser.png' width='400'>"
collection: portfolio
date: 2021-01-20

---


I designed an object-oriented “eraser” for images and videos, which is able to remove the pixels with designated types. MRCNN is used to detect the pixels of a specific type, and then WGAN is used to remove the detected pixels and restore the background.

- Demo for Image
<p align="center"><img src="/figures/Slide3.PNG" width="400" class="inline"/></p>

- Demo for Video
<p align="center">
<img src="/figures/clip1_borded.gif" width="250" class="inline"/>
<img src="/figures/clip1_erased.gif" width="250" class="inline"/></p>


- Side Project
- Code: [[Python]](https://github.com/Xiaoyang-Rebecca/DeepEraser)(Keras)


*This is a prototype only using COCO pretrained weights without any additional training.

---
<!-- << [Back](../) -->

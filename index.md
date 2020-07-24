| <img src="./figures/Rebeccca%20li.jpg" width="800">  |    Howdy, I am Rebecca Li, a Ph.D. at University of Houston with 5-yr experienced Data Scientist. I have published in top-tier AI conferences (NIPS, MICCAI) and Journals on Deep Learning, Machine Learning, and Computer Visions. My diverse advanced industrial domain experience includes Medical Image, Self-driving Car and Oil & Gas. I have greet impassion on solving real-world problem. |
-------|---------------



- E-mail:   [Xiaoyang.Rebecca.Li@gmail.com](Xiaoyang.Rebecca.Li@gmail.com)    
- [Resume](https://drive.google.com/file/d/1GApBS2kuk6vfx0mnKjPG59URymvI3xze/view) \|  [LinkedIn](http://linkedin.com/in/xiaoyang-rebecca-li) \| [ ResearchGate](http://researchgate.net/profile/Xiaoyang_Li14) \| [ GitHub](http://github.com/Xiaoyang-Rebecca ) 

--------------
# Professional Projects
--------------
## 0-Human-effort Segmentation 

We innovatively proposed an efficient unsupervised learning framework to segment nuclei robustly without the need of human annotations. We first use an iterative training process to improve segmentation quality without human labels. Then we introduce a background boosting technique to enhance the segmentation accuracy. We achieved high fidelity segmentation especially among crowed objects, and IoU improved by 3% compared to original MRCNN

<p align="center"><img src="./figures/Segmentation.PNG" width="500" class="inline"/></p>

- Advisors: Badri Roysam, Hien Nguyen
- Datail: [[Grace Hopper Celebration 2020 Poster]](https://www.researchgate.net/publication/342663998_Toward_Zero_Human_Efforts_Iterative_Training_Framework_for_Noisy_Segmentation_Label "Grace Hopper Celebration Poster"),[[Python]](https://github.com/RoysamLab/whole_brain_analysis) (Keras), partically released

--------------
## Compressive Image Recovery 

Seismic image acquisition can be time and economic costs. We adopted an appropriately designed Wasserstein generative adversarial network on compressed seismic image recovery. We first trained a pixel inpainting network on several historical surveys, and then propose a non-uniform sampling recommendation based on the evaluation of reconstructed seismic images and metrics. Our results show approximately 300 times faster than the conventional method, and better seismic reconstruction accuracy than the original GAN network.

<p align="center"><img src="./figures/Seismic_Compression.png" width="400" class="inline"/></p>

- Mentors:  Nikolaos Mitsakos, Ping Lu
- Detail: [[NIPS 2019 Workshop Poster]](https://openreview.net/forum?id=Hyleh7hqUH) , [[The leading Edge Jornal]](https://www.researchgate.net/publication/337686701_Seismic_compressive_sensing_by_generative_inpainting_network_Toward_an_optimized_acquisition_survey) 
- PythonCode(Pytorch) 


--------------
# [Related Projects]("./related_projects.md")
--------------
## DeepEraser

I designed an object-oriented "eraser" for image and video , which is able to remove the pixels with designated type. MRCNN is used to dected the pixel of a specific types then WGAN is used to remove the detect pixels as they never exist. 

<p align="center"><img src="./figures/Slide3.PNG" width="400" class="inline"/></p>
*This is just a prototype use COCO pretrained weights without any additional training.

- Demo for Video
<p align="center">
<img src="./figures/clip1_borded.gif" width="250" class="inline"/>
<img src="./figures/clip1_erased.gif" width="250" class="inline"/></p>

- [[PythonCode]](https://github.com/Xiaoyang-Rebecca/DeepEraser)

## Pixel Translator 

We use cGAN to fillin the synthetic colors on gray images of border/vein. And evaluated the reconstruction accuracy by leaf types classification using Alexnet CNN
Protopytpe of generate fake image from hand-drawn vein has also been proposed.

<p align="center"><img src="./figures/leaf.png"  width="500" class="inline"/></p>

-[[Report]](https://www.researchgate.net/publication/343178751_Synthetic_Leaf_generation_using_Conditional_Adversarial_Networks_and_classification_with_CNN), [[PPT]](https://www.researchgate.net/publication/325156994_Synthetic_Leaf_generation_using_Conditional_Adversarial_Networks_and_classification_with_CNN?ev=project),[[PythonCode]]("https://github.com/Xiaoyang-Rebecca/PixelTranslator")(Tensorflow)


## Feature Reduction to Classifier

The case study revealed the influence of 4 common feature redution methods (PCA,LDA and their kernel versions) to 4 diffenrent types of classifier (SVM, ML, KNN, GMM). Our experiments shows that SVM performed the most robust to the increasing of dimensional space, and SVM and LDA are more sensitive to noises.

<p align="center"><img src="./figures/fselection.png"  width="500" class="inline"/></p>

- [[Report]](https://www.researchgate.net/publication/308927930_Comparison_of_Feature_Reduction_Approaches_and_Classification_Approaches_for_Pattern_Recognition), [[MatlabCode]]("https://github.com/Xiaoyang-Rebecca/PatternRecognition_Matlab")


## Pick-up Drop-off Design

We use reinforcement learning to design a route from the agent so that it could use the least steps to send all the blocks to drop-off cells. A basic Q learning method has deploit. We also designed a  visulization module to display the Q values in real-time.

<p align="center"><img src="./figures/pd.gif"  width="500" class="inline"/></p>

- [[Report]](https://www.researchgate.net/publication/310607210_Learning_Paths_from_Feedback_Using_Q-Learning_for_PD_world), [[C++/matlab code]]("https://github.com/Xiaoyang-Rebecca/Artificial-intelligent")


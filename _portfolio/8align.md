---
title: "Large Scale Image Registration acceleration"
#excerpt: "Accelerated large-scale image alignment by 10× with uniform keypoint control and multiprocessing>"
collection: portfolio
---

Image registration is to align a pair of misaligned images together. Traditional image alignment algorithms involves keypoint extraction, matching and transform estimation. However, they generally failed on large scale image as the matched keypoints are not distributed correlatively with the image texture. Based on that, we proposed a tile-based feature extraction method for large scale textured based image, which enforces a uniformed sampling of key point detection on all region of the images. Also we used parallel programming to acceleration the speed of descriptors' extraction and matching by 10 times compared to the traditional method.


<p align="center"><img src="/figures/align.png"  width="550" class="inline"/></p>

**Keywords**: image registration, tile, parallel programming, multiprocessing,keypoint extraction

- Summer Internship Project at NINDS,Nation Institute of Health
- Advisors: [Dr.Badri Roysam](http://www.ee.uh.edu/faculty/roysam), [Dr.Dragan Maric](https://neuroscience.nih.gov/ninds/Faculty/Profile/dragan-maric.aspx)

- Code: [[C++/matlab]]("https://github.com/Xiaoyang-Rebecca/Artificial-intelligent")



---
<!-- << [Back](../) -->

# A Challenge for InstaHide

*InstaHide*[1] is a practical instance-hiding method for image data encryption in privacy-sensitive distributed deep learning. 

*InstaHide* uses the *Mixup*[2] method with a one-time secret key consisting of a pixel-wise random sign-flipping mask and samples from the same training dataset (Inside-dataset *InstaHide*) or a large public dataset (Cross-dataset *InstaHide*). It can be easily plugged into an existing distributed learning pipeline, and is very efficient and incurs minor reduction on accuracy. 


We release this challenge to further investigate the security of *InstaHide*.

## Citation
If you participate in this challenge or use InstaHide in your research, then please cite our paper:
```
@inproceedings{hsla20,
    title = {InstaHide: Instance-hiding Schemes for Private Distributed Learning},
    author = {Yangsibo Huang and Zhao Song and Kai Li and Sanjeev Arora},
    booktitle = {Internation Conference on Machine Learning (ICML)},
    year = {2020}
}
```

## Leaderboard
| Attack  | Submitted by  |  Success Rate | Time Complexity | Self-Reported Running Time |Submission Date  |
|---|---|---|---| ---| --- |
<!-- |   |   |   |   |   | -->

## Format and Rules

This challenge aims to find attacks that are effective against the *InstaHide* protocol.

We encourage any interested teams to propose approaches to recover private images from their *InstaHide* encryptions. 

### The Challenge Dataset

[The challenge dataset](https://www.dropbox.com/sh/8zdsr1sjftia4of/AAA-60TOjGKtGEZrRmbawwqGa?dl=0) contains 5,000 images. It is generated using 100 private images, with each image getting 50 different cross-dataset *InstaHide* encryptions using ImageNet[3] as the public dataset. k, the number of samples got mixed in each encryption, is 6 (i.e. we mix 2 private images with 4 public images).

We preprocess ImageNet in two steps to obtain public images for mixing. First,  we randomly crop a number of patches from each image in ImageNet.  Then, to filter out the "flat" patches, we design a filter using SIFT[4], a feature extraction technique to retain patches with more than 40 key points.

Images in the challenge dataset are of size 32\*32\*3, labeled from 0 to 4,999 and are in jpg format.


### Submitting Your Attack

To submit your attack, please email a zipped folder to instahide.challenge@gmail.com, which includes
- A description of the attack, including the algorithm, the analysis of time complexity, and the real-world running time (with a specification of the computation platform being used)
- The source code for the attack
- 5,000 recovered images (in jpg format). Please save the results of your attack using this naming convention: `i_recover.jpg` is the recovery of `i.jpg`


The performance of attack is evaluated by its success rate over 5,000 encryptions: a single recovery is said to be successful if the structural similarity index measure (SSIM) between the recovered image and its corresponding private image is larger than 0.8. We use `skimage.metrics.structural_similarity` (see documentation [here](https://scikit-image.org/docs/stable/api/skimage.metrics.html#skimage.metrics.structural_similarity)) to compute the mean structural similarity index between two images.

We will run evaluations and update the leaderboard after receiving your submission. 

If you have any questions, please feel free to either create an "issue" in this repository, or email to instahide.challenge@gmail.com. 

## Related Repositories
- [InstaHide](https://github.com/Hazelsuko07/InstaHide)
  
## References:
[1] [**InstaHide: Instance-hiding Schemes for Private Distributed Learning**](http://arxiv.org/abs/2010.02772), *Yangsibo Huang, Zhao Song, Kai Li, Sanjeev Arora*, ICML 2020

[2] [**mixup: Beyond Empirical Risk Minimization**](https://arxiv.org/abs/1710.09412), *Hongyi Zhang, Moustapha Cisse, Yann N. Dauphin, David Lopez-Paz*, ICLR 2018

[3] [**ImageNet: A Large-Scale Hierarchical Image Database.**](http://www.image-net.org/papers/imagenet_cvpr09.pdf), *Jia Deng, Wei Dong, Richard Socher, Li-Jia Li, Kai Li, Li Fei-Fei*, CVPR 2009

[4] [**Distinctive Image Features from Scale-Invariant Keypoints**](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf), *David G. Lowe*, IJCV 2004
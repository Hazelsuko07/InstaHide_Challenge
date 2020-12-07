# A Challenge for InstaHide

**Updated Oct 19, 2020**: We release the labels along with the encryption data. We also update the metric.

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
| Attack   |  Max_SSIM | Mean_SSIM | Min_L2 | Mean_L2 | Time Complexity | Self-Reported Running Time | Submission Date  |
| --- | --- | --- | --- | --- | --- | --- | --- |
|[Carlini et al.](https://arxiv.org/abs/2011.05315) ver.1\*|0.90|0.43| 0.01| 0.31 | O(E^3) | 2 GPU hours + 2 CPU hours (10 GPU hours for a preparation step) | 20/12/04 |
|[Carlini et al.](https://arxiv.org/abs/2011.05315) ver.2|0.79|0.37| 0.05| 0.42 | O(E^3) | 2 GPU hours + 2 CPU hours (10 GPU hours for a preparation step) | 20/12/04 |
|[PRNG Attack\**](https://arxiv.org/abs/2011.05315)|0.98|0.47| 0.00| 0.27 | O(max(E,d)^3) | 120 CPU hours | 20/11/10 |

E is the number of encryptions (5000 in the current challenge); d is the sample dimension (3072 in the current challenge).

\* The final processing step will be described in an upcoming paper.
\** This attack exploits an implementation bug of the challenge set, which is not fundamental to the InstaHide approach.

## Format and Rules

This challenge aims to find attacks that are effective against the *InstaHide* protocol.

We encourage any interested teams to propose approaches to recover private images from their *InstaHide* encryptions. 

### The Challenge Dataset

[The challenge dataset](https://www.dropbox.com/sh/8zdsr1sjftia4of/AAA-60TOjGKtGEZrRmbawwqGa?dl=0) contains 5,000 images and the random coefficients of private images. It is generated using 100 private images, with each image getting 50 different cross-dataset *InstaHide* encryptions using ImageNet[3] as the public dataset. k, the number of samples got mixed in each encryption, is 6 (i.e. we mix 2 private images with 4 public images).

We preprocess ImageNet in two steps to obtain public images for mixing. First,  we randomly crop a number of patches from each image in ImageNet.  Then, to filter out the "flat" patches, we design a filter using SIFT[4], a feature extraction technique to retain patches with more than 40 key points.

Images in the challenge dataset are of size 32\*32\*3. All encrypted images are saved as a 5000\*32\*32\*3 numpy array in `encryption.npy`, where each row represents a single encryption. `label.npy` gives the corresponding labels of encrypted images. We also provide `gen_data.ipynb`, a deterministic jupyter notebook for future verification, which takes the private dataset, the public dataset, and a random seed as input, and outputs the challenge dataset.

### Submitting Your Attack

To submit your attack, please email a zipped folder to instahide.challenge@gmail.com, which includes
- A description of the attack, including the algorithm, the analysis of time complexity, and the real-world running time (with a specification of the computation platform being used)
- The source code for the attack
- 5,000 recovered images as a 5000\*32\*32\*3 numpy array (named as `decryption.npy`), where each row represents the recovered image of its corresponding encryption in `encryption.npy`.


Let's name the two private images hidden in the encryption as `private1` and `private2`, and the recovered image by the attack as `decryption`. The performance of an attack is evaluated by the **mean** of four metrics rate over 5,000 encryptions: 
- **Max_SSIM**: Max(SSIM(decryption, private1), SSIM(decryption, private2)) 
- **Mean_SSIM**: Mean(SSIM(decryption, private1), SSIM(decryption, private2)) 
- **Max_L2**: Max(L2_diff(decryption, private1), L2_diff(decryption, private2)) 
- **Mean_L2**: Mean(L2_diff(decryption, private1), L2_diff(decryption, private2)) 

where 'SSIM' is the structural similarity index measure (see documentation [here](https://scikit-image.org/docs/stable/api/skimage.metrics.html#skimage.metrics.structural_similarity)), and 'L2_diff' is the L2 difference. We will use the first metric for ranking and the other three for reference.

We will run evaluations and update the leaderboard after receiving your submission. 

If you have any questions, please feel free to either create an "issue" in this repository, or email to instahide.challenge@gmail.com. 

## Acknowledgement
We would like to thank Nicholas Carlini for helpful discussions.

## Related Repositories
- [InstaHide](https://github.com/Hazelsuko07/InstaHide)

## References:
[1] [**InstaHide: Instance-hiding Schemes for Private Distributed Learning**](http://arxiv.org/abs/2010.02772), *Yangsibo Huang, Zhao Song, Kai Li, Sanjeev Arora*, ICML 2020

[2] [**mixup: Beyond Empirical Risk Minimization**](https://arxiv.org/abs/1710.09412), *Hongyi Zhang, Moustapha Cisse, Yann N. Dauphin, David Lopez-Paz*, ICLR 2018

[3] [**ImageNet: A Large-Scale Hierarchical Image Database.**](http://www.image-net.org/papers/imagenet_cvpr09.pdf), *Jia Deng, Wei Dong, Richard Socher, Li-Jia Li, Kai Li, Li Fei-Fei*, CVPR 2009

[4] [**Distinctive Image Features from Scale-Invariant Keypoints**](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf), *David G. Lowe*, IJCV 2004

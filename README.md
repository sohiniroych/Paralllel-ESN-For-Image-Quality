# Few Shot Learning Framework to Reduce Inter-observer Variability in Medical Images

This work is related to the ICPR 2020 paper titled "Few Shot Learning Framework to Reduce Inter-observer Variability in Medical Images"

---

## Overview
This paper presents 3 ways to learn semantic segmentation for images using as few as 5 medical images to train on. The three methods are:
1. Global Thresholding (Baseline)
2. Parallel ESN
3. U-net

Once trained, the models predict three different versions of regions of interest for each test image. These three images are called proposals (P1, P2, P3).
The next step is a Target Label Selection Algorithm (TLSA)(Algorithm 1 in paper) to select the best manual annotation per test images, given two manual annotations (U1, U2).

## Target Label Selection Algorithm (TLSA)

This algorithm uses the following as input:
1. Proposals: P1, P2, P3
2. Manual Annotations: U1, U2

Output is a decision={0,1,2}. 0=manual intervention needed. 1=U1 is best annotation, 2=U2 is best annotation.

### Metrics Implementation

The implementation details for the metrics to be computed for TLSA are as follows:
1. Intersection over Union (IoU) between (P1,P2)(P2,P3)(P1,P3), and compute variance over mean for these 3 combinations. High value implies high disagreeability between proposals and manual intervention is needed (A).
2. IoU between combinations: (P1,U1)(P2,U1)(P3,U1). Pick the combination with highest IoU and note the proposal index and the max IoU (B). 
3. IoU between combinations: (P1,U2)(P2,U2)(P3,U2). Pick the combination with highest IoU and note the proposal index and the max IoU (C). 
4. Ratio_u=(B)/(C). Higher better. 
5. Extract image I' by multiplying image I with the target label U1 and all the proposal areas (P1 U P2 U P3). Compute mean and standard deviations of all pixels in image I'. Compute variance over mean for pixel values in I' (D).
6. Extract image I' by multiplying image I with the target label U2 and all the proposal areas (P1 U P2 U P3). Compute mean and standard deviations of all pixels in image I'. Compute variance over mean for pixel values in I' (E). 
7. Ratio_v=(D)/(E). Lower better.


### TLSA Implementation

1. Verify if U1 and U2 are blank (sum of all pixels=0). If so return decision=0.
2. If U1 and U2 are not blank, check if they have very small deviations from each other or not. If U1 and U2 have less than 20 pixels difference between them and if metric (A) is high, return decision =0. [Since proposals have high variability and target labels are very similar].
3.If U1 and U2 are significantly dissimilar, compute Ratio_u. If Ratio_u>1.1, decision =1, elseif Ratio_u<1/1.1, decision=2
4. If 0.91>Ratio_u>1.1, compute Ratio_v. If Ratio_v>1.02, decision =1, elseif Ratio_v<1/1.02, decision=2.
5. If 0.98>Ratio_v>1.02, decision=0.

### Examples

Refer to the 4 sample images, their sample annotations and proposals in folder Samples.
Image 1:img\1.jpg

Here, Ratio_u is the decisive metric to return decision=1
Image 2:img\2.jpg

Here, Ratio_v is the decisive metric to return decision=2


Image 3:img\3.jpg


Here, Ratio_u is the decisive metric to return decision=1



Image 4:img\4.jpg

Here, there is zero overlap between proposals and target labels, thus decision=0

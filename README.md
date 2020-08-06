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

### Target Label Selection Algorithm (TLSA)

This algorithm uses the following as input:
1. Proposals: P1, P2, P3
2. Manual Annotations: U1, U2

Output is a decision={0,1,2}. 0=manual intervention needed. 1=U1 is best annotation, 2=U2 is best annotation.

### Metrics

The metrics to be computed for TLSA are as follows:
1. 

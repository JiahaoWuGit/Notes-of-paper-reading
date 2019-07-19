# 0. My own view:  

This is a paper which is mainly about 

- how to deal with unlabeled dataset(about images) and make them to become useful, contributing to improve the performance  of models trained by these data.

# 1. Introduction

limited annotated training data 

- => make use of unlabeled data 
- =>  way of use unlabeled data: cluster them 
- => unsupervised methods, such as K-means ..... 
- => problem of these methods is that they rely on simplistic assumptions
- => Consequently, they lack the capability of coping with complicated cluster structures,thus often giving rise to noisy clusters, especially when applied to large-scale datasets collected from real-world settings. This problem seriously limits the performance improvement.
- =====================>>>>>>>>>>>>>>>>>>>
- need to develop an effective clustering algorithm that is able to cope with the complicated cluster structures 
- ==========================>>>>>>>>>>>>>>>>>
- In this work, they explore a fundamentally different approach, that is, to learn how to cluster from data. Particularly, they desire to draw on the strong expressive power of graph convolutional network to capture the common patterns in face clusters, and leverage them to help to partition the unlabeled data. 

# 2. This paper's way standing out of Related work

### Face clustering:

Other work still rely on unsupervised methods to perform clustering. *This paper's method learns how to cluster in a top-down manner, based on a detection-segmentation paradigm*. **Advantages of this method**  is this allows model to handle clusters with complicated structures.

### Graph Convolutional Networks

Adopt GCN as the basic machinery to capture c luster patterns on an affinity graph. This is  the first work that uses GCN to learn how to cluster in a supervised way.

# 3. Methodology

To tackle the complex variations of the cluster patterns, this paper explore a supervised approach: learn the cluster patterns based on graph convolutional networks. And formulate this as a joint detection and segmentation problem on an afﬁnity graph. 

### Framework Overview

The whole process has three phase:

- proposal generator:	generates cluster proposals
- GCN-D: perform cluster detection and select high-quality proposals
- GCN-S: refine the selected proposals by removing the noises therein and prunes the cluster by discarding the outliers.

### Cluster Proposals(inspired by the cited paper *6,7*)

-  Advantages: reduce the computational cost, since only a limited number of cluster candidates need to be evaluated.
- steps:
  - generate super-vertex:
    - super-vertex is a sub-graph containing a small number of vertices that are closely connected to each other.
  - initiate an algorithm to compose cluster proposals based on super-vertices

### Cluster Detection => Cluster Segmentation => De-Overlapping

# 4. Experiment

### 4.1 Experimental Settings

- Training set:
  - face recognition dataset:	
    -  MS-Celeb-1M***[Cited paper 1]*** ,consists of 100K identities and each identity has about 100 facial images. 
    - Because of being obtained from webpages directly, they are noisy. Therefore,  this paper cleans this dataset based on the annotations from ArcFace***[Citd paper 3]*** , generating a new subset that contains 5.8M images from 86K classes.
    - The cleaned dataset is randomly split into 10 parts equally.  Each part contains 8.6K identities with around 580K images
    - How to use them:   randomly select 1 part as labeled data and the other 9 parts as unlabeled data
  - evaluation dataset:
    -  Youtube Face Dataset ***[Cited paper 28]*** contains 3,425 videos, from which this paper extracts 155,882 frames for evaluation
    -  use 14,653 frames with 159 identities for training and the other 140,629 images with 1,436 identities for testing. 
- Testing set:
  -  MegaFace***[Cited paper14]***is the largest public benchmark for face recognition. It includes a probe set from FaceScrub ***[Cited paper21]*** with 3,530 images and a gallery set containing 1M images.  
  - IJB-A ***[Cited paper16]*** is another face recognition benchmark containing 5,712 images from 500 identities.
- Metrics(evaluation):
  - assess performance on two task:
    - face clustering:
      - what to do :	cluster all the images of the same identity into a cluster
      - what to evaluate:     measure F-score to evaluate both of pairwise recall and pairwise precision
    - face recognition:
      - **face identiﬁcation** benchmark in MegaFace and **face veriﬁcation** protocol of IJB-A. 
      - For identification:  adopt top-1 identiﬁcation hit rate in MegaFace, which is to rank the top-1 image from the 1M gallery images and compute the top-1 hit rate. 
      - For verification:  adopt the protocol of face veriﬁcation, which is to determine whether two given face images are from the same identity
      - Caution: use true positive rate under the condition that the false positive rate is 0.001 for evaluation.

### 4.2 Method Comparison

#### 4.2.1 Face Clustering

- Compare with K-means, DBSCAN, HAC, approximate rank order, CDP, GCN-D, GCN-D + GCN-S

- To control the experimental time, they randomly select one part of the data for evaluation, containing 580K images of 8,573 identities. Tab. 1 compares the performance of different methods on this set. The clustering performance is evaluated by both F-score and the time cost. We also report the number of clusters, pairwise precision and pairwise recall for better understanding the advantages and disadvantages of each method. 

#### 4.2.2 face recognition

- purpose : With the trained clustering model, we apply it to unlabeled data to obtain pseudo labels. We investigate how the unlabeled data with pseudo labels enhance the performance of face recognition
- steps:
  -  (1) train the initial recognition model with labeled data in a supervised way;
  -  (2) train the clustering model on the labeled set, using the feature representation derived from the initial model;
  -  (3) apply the clustering model to group unlabeled data with various amounts (1, 3, 5, 7, 9 parts), and thus attach to them “pseudo-labels”; and 
  - (4) train the ﬁnal recognition model using the whole dataset, with both original labeled data and the others with assigned pseudo-labels. 

### 4.3 Ablation Study

 randomly select one part of the unlabeled data, containing 580K images of 8,573 identities,to study some important design choices in framework. 

#### 4.3.1 Proposal Strategies 

Determine the iteration steps and numbers of proposals to the get best F-score***( trade-off between performance and computational cost in choosing the proper number of proposals.)*** 

#### 4.3.2 Design choice of GCN-D 

#### 4.3.3 GCN-S 

#### 4.3.4 Post-processstrategies 

# 5. Basic Knowledge I need to learn  while reading this paper:

- what is GCN and CNN? How do they work?
- read the cited paper *6,7* 
- what is F-score?


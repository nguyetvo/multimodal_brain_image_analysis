# Multimodal Brain Tumor Segmentation 

## IDEA
- Use techniques of previous research
- Design new loss function (using the idea of previous paper)
- Reduce size of models
- Handle the open problems of previous research
- Design new model: cascaded, because they are overlap


## Documents
- Data: 
    - http://braintumorsegmentation.org/ (all BRatS data)
    - https://github.com/BraTS/Instructions/blob/master/data_formats.md
- Docker from previous implementation:
    - hub.docker.com/u/brats/
- Documents:
    - From https://www.med.upenn.edu/cbica/brats2019/registration.html
        - https://arxiv.org/pdf/1811.02629.pdf
        - https://www.ncbi.nlm.nih.gov/pubmed/25494501
        - https://www.ncbi.nlm.nih.gov/pubmed/28872634
        - https://wiki.cancerimagingarchive.net/display/DOI/Segmentation+Labels+and+Radiomic+Features+for+the+Pre-operative+Scans+of+the+TCGA-GBM+collection
        - https://wiki.cancerimagingarchive.net/display/DOI/Segmentation+Labels+and+Radiomic+Features+for+the+Pre-operative+Scans+of+the+TCGA-LGG+collection
    - From other sites:
        - https://paperswithcode.com/task/brain-tumor-segmentation
        - https://www.frontiersin.org/articles/10.3389/fncom.2019.00083/full
        - BRATS 2018:
            - https://arxiv.org/pdf/1810.11654v3.pdf (top 1)
            - https://link.springer.com/chapter/10.1007%2F978-3-030-11726-9_21 (top 2)
            - https://link.springer.com/content/pdf/10.1007%2F978-3-030-11726-9_40.pdf (top 3)
            - https://link.springer.com/content/pdf/10.1007%2F978-3-030-11726-9_44.pdf (top 3)
            - others: https://link.springer.com/book/10.1007/978-3-030-11726-9

- Github:
    - https://github.com/JooHyun-Lee/BraTs
    - https://github.com/JunMa11/SOTA-MedSeg

## Literature Review
|                                 Paper                                |                                                                                                             What's new?                                                                                                             | Methods                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    What have we learned?                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                               Open problems?                                                                                                                                                              |
|:--------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|--------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| 3D MRI brain tumor segmentation using <br>autoencoder regularization | Due to a limited training dataset size, <br>a variational auto-encoder branch is added <br>to reconstruct the input image itself in order to <br>regularize the shared decoder and<br> impose additional constraints on its layers. | Input: 160x192x128<br>Outputs: output all 3 nested tumor subregions directly after the sigmoid<br><br><br>Using UNET based + VAE to segmentation | - Add new VAE branch: using the auto-encoder branch is to add additional guidance and regularization to the encoder part, since the training dataset size is limited<br>- For normalization, we use Group Normalization (GN), which shows better than BatchNorm performance when batch size is small<br>- Ldice is a soft dice loss [19] applied to the decoder output ppred to match the segmentation mask ptrue<br>- We have also experimented with more sophisticated data augmentation techniques, including random histogram matching, affine image transforms, and random image filtering, which did not demonstrate any additional improvements.<br>- We have tried several data post-processing techniques to fine tune the segmentation predictions with CRF [14], but did not find it beneficial,<br>- Increasing the network depth further did not improve the performance, but increasing the network width (the number of features/filters) consistently improved the results | - The size of model is too large, require 2 days of V100 32G to train with batchsize 1 (300 epochs)<br>-The additional VAE branch helped to regularize the shared encoder (in presence of limited data), which not only improved the performance, but helped to consistently achieve good training accuracy for any random initialization |
|                                                                      |                                                                                                                                                                                                                                     |                                                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                           |

## Application of Medical Image Overview
I do a summarization of application from MICCAI 2019 papers [here](./research/application_medical_overview.md)

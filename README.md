# Project: Object Detection in Overhead Remote Sensing Imagery

## Contents

- [Group Members](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#group-members)
- [Project Overview](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#project-overview)
- [Dataset(s)](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#datasets)
- [Baseline](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#baseline)
- [Improvement 1](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#improvement-1)
- [Improvement 2](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#improvement-2)
- [Results](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#results)
- [How to Run](https://github.com/CS-437-Spring-2026-Project-Group-17/Object-Detection-in-Overhead-Remote-Sensing-Imagery#how-to-run)

## Group Members
- Muhammad Yahya Ali (27100160)
- Abdullah Ahmad (27100387)

## Project Overview

Identification of tiny objects in overhead imagery from drones and satellites is a critical challenge which has practical applications in traffic observation, ship tracking, and disaster response. The core difficulty is threefold: objects can exist with dimensions as small as 8×8 pixels, busy backgrounds (such as harbors, roads, tree canopies etc.) are oftenly mistaken with actual targets, and aerial datasets that are annotated in an expert manner remain limited and expensive to produce.

Current approaches each address only one of these problems at a time. LGA-YOLO and AUHF-DETR enhance small-object accuracy but need completely annotated data, ShadowFPN-YOLO attains fast inference but overlooks label scarcity and B-FSDet addresses few-shot settings but hasn't undergone testing on drone hardware. No single model tackles all three at the same time.

We suggest a lightweight detector based on YOLOv8m that implements an approach of background-masking training (to stop the model from relying on adjacent clutter), a mechanism of class weighting (to counter the problem of class imbalance) and another layer of detection called P2 head (to get better detection of small objects). The model is evaluated on **VisDrone2019** under both full-data and few-shot regimes. Precision, recall, mAP@0.5, mAP@0.5:0.95, FPS, parameter count and GFLOPs were used as metrics for evaluation.

## Dataset(s)

Kaggle link: [Dataset_DL_Proj_Spring26](https://www.kaggle.com/datasets/abd915/dataset-dl-proj-spring26)

Note: This uses VisDrone2019 dataset (which is a public dataset), we do not claim any datasets as our own and all rights belong to their original owners. Following is the link of this dataset that we used for reference:

[https://datasetninja.com/vis-drone-2019-det](https://datasetninja.com/vis-drone-2019-det)

## Baseline

YOLOv8m trained on VisDrone2019 (converted to YOLO format) using pretrained weights, 20 epochs, image size 640, batch size 8.

## Improvement 1

**Baseline + Masking:** During training process, background patches chosen on random basis were hidden (applied with probability 0.5, 4–8 patches per image, each patch covering 3–10% of image dimensions) while making sure that object bounding boxes remained protected. This prompted the model to focus on characteristics of object instead of background signals such as roads and shadows.

**Baseline + Class Weighting:** Classes that occur very less in terms of frequenct (such as awning-tricycle, tricycle, bus etc.) were provided greater emphasis during training to handle the class imbalance issue of the dataset. This made the model consider mistakes on rare classes as more significant, prompting it to identify more actual objects.

## Improvement 2

**Baseline + P2 Head:** One more detection layer (P2) that is higher in resolution was incorporated to the YOLOv8m architecture so that the model is given with more detailed feature maps for identifying tiny objects. Although precision jumped from 0.521 to 0.544, recall dropped to 0.409 and FPS fell from 48.70 to 40.61 as an evident increase in computational expense was observed (there was a rise in GFLOPs from 79.32 to 99.00).

**Baseline + All (Masking + Class Weighting + P2 Head):** All three improvements were combined to check whether they work more effectively as a group. The hybrid model attained the highest precision (0.545) but the lowest recall among all other implementations (0.402) and the lowest mAP@0.5:0.95 (0.235) were noted, implying that it became too much cautious. This resulted in predictions that were less incorrect but overlooking more actual objects.

## Results

| Metric | Baseline | Baseline + Masking | Baseline + Class Weighting | Baseline + P2 Head | Baseline + All |
|---|---|---|---|---|---|
| Precision | 0.521 | 0.543 | 0.539 | 0.551 | 0.545 |
| Recall | 0.414 | 0.419 | 0.425 | 0.403 | 0.402 |
| mAP@0.5 | 0.406 | 0.415 | 0.414 | 0.404 | 0.397 |
| mAP@0.5:0.95 | 0.240 | 0.245 | 0.246 | 0.240 | 0.235 |
| FPS | 48.77 | 49.03 | 48.88 | 39.93 | 39.84 |
| Parameters | 25902640 | 25862110 | 25862110 | 25082496 | 25082496 |
| GFLOPs | 79.32 | 79.10 | 79.10 | 99.00 | 99.00 |

**Notes:**

- **Baseline + Masking:** Masking resulted in improvement of all identification metrics with no evident jump in terms of computation cost.
- **Baseline + Class Weighting:** Implementations of weights to each class resulted in the greatest recall and mAP@0.5:0.95 among all other models that were evaluated, establishing it as the best overall improvement.
- **Baseline + P2 Head:** P2 was unable to improvem overall identification in this setup, probably needing more training or greater image size to realize its full potential.
- **Baseline + All (Masking + Class Weighting + P2 Head):** Combining all improvements did not exceed the performance of the individual techniques, confirming that each improvement should be evaluated independently.

## How to Run

- Clone this repository to your local machine so that the you have the notebook file on local machine.
- Upload the notebook on kaggle
- Go to edit notebook. In the inputs section, paste the kaggle link mentioned in the dataset(s) section to the search bar of input. You should see Dataset_DL_Proj_Spring26 dataset. Add that as the input.
- Select the GPU T4 as accelerator and run the notebook file.
- Note: Running time of the notebook is approximately 9 to 10 hours on GPU T4

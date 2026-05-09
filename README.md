# Project: Object Detection in Overhead Remote Sensing Imagery

## Group Members
- Muhammad Yahya Ali (27100160)
- Abdullah Ahmad (27100387)

## Project Overview

Identification of tiny objects in overhead imagery from drones and satellites is a critical challenge which has practical applications in traffic observation, ship tracking, and disaster response. The core difficulty is threefold: objects can exist with dimensions as small as 8×8 pixels, busy backgrounds (such as harbors, roads, tree canopies etc.) are oftenly mistaken with actual targets, and aerial datasets that are annotated in an expert manner remain limited and expensive to produce.

Current approaches each address only one of these problems at a time. LGA-YOLO and AUHF-DETR enhance small-object accuracy but need completely annotated data, ShadowFPN-YOLO attains fast inference but overlooks label scarcity and B-FSDet addresses few-shot settings but hasn't undergone testing on drone hardware. No single model tackles all three at the same time.

We suggest a lightweight detector based on YOLOv8m that implements an approach of background-masking training (to stop the model from relying on adjacent clutter), a mechanism of class weighting (to counter the problem of class imbalance) and another layer of detection called P2 head (to get better detection of small objects). The model is evaluated on **VisDrone2019** under both full-data and few-shot regimes, measuring precision, recall, mAP@0.5, mAP@0.5:0.95, FPS, parameter count and GFLOPs.

## Dataset

Kaggle link: [Dataset_DL_Proj_Spring26](https://www.kaggle.com/datasets/abd915/dataset-dl-proj-spring26)

Note: This uses VisDrone2019 dataset (which is a public dataset), we do not claim any datasets as our own and all rights belong to their original owners. Following is the link of this dataset that we used for reference:

[https://datasetninja.com/vis-drone-2019-det](https://datasetninja.com/vis-drone-2019-det)

## Baseline

## Improvement 1

## Improvement 2

## How to Run

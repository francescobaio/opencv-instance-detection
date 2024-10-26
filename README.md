# opencv-instance-detection


# 🛒 Product Recognition of Food Products

![Python](https://img.shields.io/badge/Python-3.x-blue) ![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green) ![License](https://img.shields.io/badge/License-MIT-yellow)

## 📜 Overview
This project is focused on developing a computer vision-based object detection system to identify food products displayed on store shelves. Such a system could be highly beneficial in various scenarios:
- **Assistive Technology**: Supporting visually impaired customers by helping them locate specific products on shelves.
- **Store Management**: Aiding store employees in detecting low stock, misplaced products, or performing shelf audits more efficiently.

## 🎯 Objectives
- **Single Instance Detection**: Detect individual instances of products on a shelf based on a reference image.
- **Multiple Instance Detection**: Extend detection to multiple occurrences of the same product within the same scene.

## 📂 Data Preparation and Preprocessing

The dataset consists of two primary folders of images:

- **Models**: This folder contains reference images of individual products. Each product type has one reference image that the system uses to detect corresponding instances within the shelf images.
- **Scenes**: This folder includes images of store shelves with multiple products, often corrupted by significant noise, simulating real-world conditions like lighting variability, reflections, and artifacts. These noisy images pose challenges for accurate detection and require preprocessing to enhance the system’s reliability.

### 🧼 Preprocessing Steps

To improve detection accuracy, the images undergo a preprocessing pipeline with the following steps:

1. **Median Filter**: A 5x5 kernel is applied to each image to remove salt-and-pepper noise, a common type of high-frequency noise that introduces small, random bright or dark spots. 

2. **Bilateral Filter**: This filter reduces Gaussian-like noise while preserving edges, which is essential for maintaining the structural details of each product. A high `sigmaColor` value is used to emphasize color consistency in regions with similar intensities, helping the model focus on the color information despite noise. This step is particularly useful for the scenes, where color and edge information must be balanced carefully to avoid false matches.

These preprocessing steps improve the feature matching process by enhancing key product details and reducing irrelevant noise, thereby increasing the accuracy and robustness of the detection pipeline.

## 🔨 Solution Approach

### 1️⃣ Single Instance Detection

This process is designed to detect a single occurrence of each product in the shelf image. The main steps include:

- **🔍 SIFT Feature Detection**: The Scale-Invariant Feature Transform (SIFT) algorithm is used to detect keypoints and compute descriptors in both the product reference images (models) and the shelf images (scenes). This provides robust feature matching by identifying structural details in the images.
  
- **🔨 Homography Estimation with RANSAC**: Once a sufficient number of matches is found between model and scene images, the RANSAC (Random Sample Consensus) algorithm is applied to estimate a homography matrix. This matrix allows the accurate positioning of each product in the scene by aligning corresponding keypoints from the model and scene. A high `ransacReprojThreshold` is used to ensure stability in the presence of noise or fewer matching keypoints, resulting in precise bounding box calculations.

- **🎨 Color Consistency Check**: To further verify the matches, both the product reference images and scene patches are converted to the LAB color space. This space is perceptually uniform, making it more suitable for color verification in line with human perception. Each image is divided into patches, with the patch size (`r`) based on the average keypoint diameter, which helps in capturing the correct scale of structural details. Color histograms are computed on the A and B (chroma) channels only, as these represent color information and the Kullback-Leibler divergence is calculated to measure color similarity. This ensures that only matches consistent in both shape and color are retained.

### 2️⃣ Multiple Instance Detection

To detect multiple instances of the same product, the single instance detection method is extended with:

- **🔁 Repeated Application**: The detection process is applied iteratively across the entire image. After each detection, the identified product instance is masked out of the scene. This enables the system to detect additional instances without interference from previously detected products, allowing a clean and accurate localization of each instance in the scene.
  
This extended approach ensures comprehensive detection of all occurrences of a product within the same scene image.

## 📈 Results
For each detected product, the system outputs:
- **Instance count**
- **Bounding box dimensions** (width and height)
- **Position** (center coordinates)


## 📧 Contact
- Francesco Baiocchi - francesco.baiocchi2@stud.unibo.it
- Leonardo Petrilli - leonardo.petrilli@stud.unibo.it
---

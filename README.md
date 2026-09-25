# Noise-Based Image Tampering Detection Using MATLAB

## 1. Overview

This project presents a MATLAB-based approach for extracting **noise-based features from digital images** to support image tampering detection.

The method is based on the observation that an original image generally contains consistent noise characteristics produced by the camera sensor and image-processing pipeline. A manipulated region may have different noise characteristics compared with the surrounding image.

The system extracts statistical information from image noise and generates a **13-dimensional feature vector**. These features can later be used as input to a machine-learning or AI-based classifier.

---

## 2. Objective

The main objectives of this project are:

* To extract high-frequency noise information from an image.
* To analyze the statistical properties of image noise.
* To measure noise consistency across different regions of an image.
* To generate numerical features representing the noise characteristics of an image.
* To provide a foundation for developing an AI/ML-based image tampering detection system.

---

## 3. Software Requirements

The following software is required:

* **MATLAB**
* **Image Processing Toolbox**
* MATLAB-compatible operating system

The program supports common image formats such as:

* JPG
* JPEG
* PNG
* BMP
* TIF

---

## 4. Methodology

The overall methodology of the system is:

```text
Input Image
     ↓
Convert to Grayscale
     ↓
High-Pass / SRM-Like Filtering
     ↓
Noise Residual Extraction
     ↓
Statistical Analysis
     ↓
Block-Wise Noise Analysis
     ↓
Feature Extraction
     ↓
13-Dimensional Feature Vector
```

The extracted features describe the statistical behavior and spatial consistency of the noise present in the image.

These features can subsequently be supplied to an AI/ML classifier for image tampering classification.

---

## 5. High-Pass Filtering

High-pass filters are used to suppress low-frequency image content and emphasize high-frequency components such as edges, fine details, and noise.

The program uses three simple SRM-like filters.

### Filter 1

```matlab
[-1  2 -1
  2 -4  2
 -1  2 -1] / 4
```

This filter emphasizes higher-order local variations in the image.

### Filter 2

```matlab
[0  0  0
 1 -2  1
 0  0  0]
```

This filter emphasizes horizontal intensity variations.

### Filter 3

```matlab
[0  1  0
 0 -2  0
 0  1  0]
```

This filter emphasizes vertical intensity variations.

The output of each filter is considered a **noise residual**, which is then statistically analyzed.

---

## 6. Statistical Analysis

For each of the three noise residuals, four statistical properties are calculated:

### Mean Absolute Value

Represents the average magnitude of the noise residual.

### Standard Deviation

Represents the amount of variation present in the noise.

### Skewness

Describes the asymmetry of the noise distribution.

### Kurtosis

Describes the shape and peakedness of the noise distribution.

Therefore:

```text
3 Filters × 4 Statistical Features
= 12 Features
```

These 12 features provide numerical information about the overall noise characteristics of the image.

---

## 7. Block-Wise Noise Analysis

In addition to global noise statistics, the program performs local noise analysis.

The noise residual obtained from the first high-pass filter is divided into **16 × 16 pixel blocks**.

For every block, the variance of the noise is calculated.

The variation between these block variances is then used to calculate a **noise inconsistency value**:

```matlab
std(blockVars(:)) / (mean(blockVars(:)) + eps)
```

This measures how much the noise level varies between different regions of the image.

A manipulated region may have different noise characteristics from the surrounding image because it may have originated from a different image or processing pipeline.

This produces one additional feature:

```text
Block-Wise Noise Inconsistency = 1 Feature
```

---

## 8. Feature Extraction

The final feature vector consists of:

```text
12 Statistical Features
+
1 Block-Wise Noise Inconsistency Feature
=
13 Total Features
```

The 13 features are:

```text
Filter 1
    1. Mean Absolute Value
    2. Standard Deviation
    3. Skewness
    4. Kurtosis

Filter 2
    5. Mean Absolute Value
    6. Standard Deviation
    7. Skewness
    8. Kurtosis

Filter 3
    9. Mean Absolute Value
   10. Standard Deviation
   11. Skewness
   12. Kurtosis

Block Analysis
   13. Noise Inconsistency
```

The final result is stored as a numerical feature vector:

```matlab
noiseFeatures
```

These features can be used as input to a machine-learning model.

---

## 9. How to Run

Follow these steps to execute the program:

1. Open MATLAB.
2. Create a new MATLAB script.
3. Save the program as:

```text
noiseFeatureDemo.m
```

4. Set the MATLAB Current Folder to the location containing the program.
5. Click **Run**.
6. An image-selection window will appear.
7. Select the image that needs to be analyzed.
8. The selected image will be displayed.
9. The program will extract the noise features.
10. The 13 feature values will be displayed in the MATLAB Command Window.

---

## 10. Input

The input to the system is a digital image.

The program accepts common image formats including:

* `.jpg`
* `.jpeg`
* `.png`
* `.bmp`
* `.tif`

The image can be either:

* RGB/color image
* Grayscale image

If the input is an RGB image, it is automatically converted into grayscale before feature extraction.

### Input Screenshot

**Paste the input screenshot here.**

---

## 11. Program Functions

### `noiseFeatureDemo`

This is the main program.

It performs the following operations:

* Opens the image-selection window.
* Loads the selected image.
* Displays the input image.
* Calls the feature extraction function.
* Displays the extracted features.

### `calculateNoiseFeatures`

This function performs the main feature extraction process.

It:

* Converts the image to grayscale.
* Converts image data into double precision.
* Applies three high-pass filters.
* Extracts noise residuals.
* Calculates mean absolute value.
* Calculates standard deviation.
* Calculates skewness.
* Calculates kurtosis.
* Performs 16 × 16 block-wise noise analysis.
* Calculates noise inconsistency.
* Returns the final 13-feature vector.

---

## 12. Output

The program produces two main outputs.

### 1. Displayed Image

The selected input image is displayed in a MATLAB figure window.

### 2. Extracted Noise Features

The Command Window displays the 13 extracted features:

Extracted Noise Features:
-------------------------
Feature 1 = 3.232721
Feature 2 = 12.780359
Feature 3 = 1.096061
Feature 4 = 44.674058
Feature 5 = 15.104335
Feature 6 = 52.440634
Feature 7 = 1.816807
Feature 8 = 24.368613
Feature 9 = 13.472108
Feature 10 = 49.612048
Feature 11 = 1.330678
Feature 12 = 25.243533
Feature 13 = 2.409082

Total number of features = 13

The final output is therefore a **13-dimensional numerical feature vector**.

### Output Screenshot

**Paste the output screenshot here.**

---

## 13. Applications

The extracted noise features can be used in applications such as:

* Digital image forensics
* Image tampering detection
* Image forgery analysis
* Image splicing detection
* Copy-paste manipulation analysis
* Photo authenticity analysis
* AI-based image classification
* Multimedia security

---

## 14. Limitations

The current implementation is a **noise feature extraction system** and does not directly determine whether an image is authentic or tampered.

The extracted noise characteristics can also be affected by:

* JPEG compression
* Image resizing
* Image enhancement
* Noise reduction
* Different camera sensors
* Different image-processing pipelines
* Screenshots
* Social-media compression
* Changes in image quality

Therefore, the 13 extracted features alone should not be considered a definitive indication of image tampering.

---

## 15. Future Scope

The current system can be extended into a complete **AI-based image tampering detection system**.

The future system can follow:

```text
Input Image
     ↓
Noise Feature Extraction
     ↓
13 Noise Features
     ↓
Training Dataset
     ↓
Machine Learning Model
     ↓
Classification
     ↓
Authentic / Potentially Tampered
```

The extracted features can be used with machine-learning algorithms such as:

* Support Vector Machine (SVM)
* Random Forest
* Decision Tree
* K-Nearest Neighbors (KNN)
* Artificial Neural Network (ANN)

A dataset containing both authentic and tampered images can be created to train and evaluate the classification model.

Future improvements can also include:

* Increasing the number of noise filters.
* Extracting additional texture features.
* Combining noise features with edge and texture information.
* Using deep-learning models.
* Generating a tampering probability.
* Localizing the suspected tampered region instead of only analyzing the complete image.

---

## 16. Conclusion

This project demonstrates a **noise-based image feature extraction technique for digital image tampering analysis** using MATLAB.

The system applies high-pass filters to extract noise residuals, calculates statistical properties of the residuals, and analyzes local noise consistency using 16 × 16 image blocks.

A total of **13 numerical features** are generated from the input image.

These features provide a foundation for developing a complete **AI/ML-based image tampering detection system**, where the extracted features can be further processed by a trained classifier to distinguish between authentic and potentially tampered images.

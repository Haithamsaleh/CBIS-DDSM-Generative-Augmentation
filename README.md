# CBIS-DDSM Breast Cancer Classification with Generative AI Augmentation

## Project Overview

This project evaluates the impact of **Generative AI-based data augmentation** on medical image classification using the **CBIS-DDSM breast cancer dataset**.

The experiment compares two main settings:

- **Model-A (Baseline):** trained using the original CBIS-DDSM dataset.
- **Model-B (Augmented Data):** trained using the original dataset plus synthetic images generated using a **Variational Autoencoder (VAE)**.

The goal is to determine whether generative augmentation improves the performance of traditional machine learning classifiers in binary breast cancer classification.

## Classification Task

The project uses binary classification:

| Class | Label |
|---|---:|
| Benign | 0 |
| Malignant | 1 |

Both **mass** and **calcification** cases from CBIS-DDSM were used.

## Dataset

Dataset used:

- **CBIS-DDSM Breast Cancer Image Dataset**
- Source: Kaggle
- Data type: Mammography images with CSV metadata
- Used cases: Mass and calcification cases
- Target labels: Benign and Malignant

The original CSV files used are:

```text
mass_case_description_train_set.csv
mass_case_description_test_set.csv
calc_case_description_train_set.csv
calc_case_description_test_set.csv
```

The image paths from the CSV metadata were matched with the available JPEG image files.

## Project Structure

```text
CBIS-DDSM-Generative-Augmentation/
│
├── notebooks/
│   └── IT504_CBIS_DDSM_Project.ipynb
│
├── augmented_images/
│   ├── benign/
│   │   └── benign_generated_0000.png
│   └── malignant/
│       └── malignant_generated_0000.png
│
├── results/
│   ├── model_a_vs_model_b_results.csv
│   ├── model_b_improvement.csv
│   └── best_model_summary.csv
│
├── presentation/
│   └── IT504_CBIS_DDSM_Final_Project_Presentation.pptx
│
└── README.md
```

## Methodology

### 1. Model-A: Baseline

Model-A was trained using only the original CBIS-DDSM training data.

The preprocessing and feature extraction pipeline included:

1. Loading image metadata from CSV files.
2. Matching CSV image paths with actual JPEG files.
3. Converting pathology labels into binary labels.
4. Converting images to grayscale.
5. Resizing images.
6. Applying **CLAHE** for local contrast enhancement.
7. Extracting **HOG** features.
8. Training four traditional machine learning models.
9. Evaluating performance on the original test set.

### 2. Image Preprocessing

The image preprocessing pipeline used:

- **Grayscale conversion**
- **Image resizing**
- **CLAHE**: Contrast Limited Adaptive Histogram Equalization
- **HOG**: Histogram of Oriented Gradients feature extraction

CLAHE was used to enhance local contrast in mammogram images, while HOG was used to extract edge and shape-based features suitable for traditional machine learning models.

### 3. Machine Learning Models

The following models were trained and evaluated:

- Support Vector Machine (SVM)
- Decision Tree
- Random Forest
- Naive Bayes

### 4. Threshold Tuning

Instead of using the default threshold of `0.50`, decision thresholds were tuned using the validation set to improve F1-score.

The test set was not used for threshold tuning.

### 5. Generative AI Augmentation

For Model-B, a **Variational Autoencoder (VAE)** was used to generate synthetic mammogram-like images.

Separate VAE models were trained for:

- Benign images
- Malignant images

The generated synthetic images were added only to the training set.

Validation and test sets remained unchanged to ensure a fair comparison between Model-A and Model-B.

## Model-B: Augmented Data

Model-B used:

```text
Original training data + VAE-generated benign and malignant images
```

The same preprocessing, feature extraction, models, and evaluation metrics were used for Model-B.

This allows a fair comparison between the original-data baseline and the augmented-data experiment.

## Evaluation Metrics

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- AUC
- Confusion Matrix
- ROC Curve
- Precision-Recall Curve

## Results Summary

The final comparison showed that VAE-based augmentation had a mixed impact.

Some models improved in selected metrics after augmentation, while others showed reduced or unchanged performance.

Main observations:

- Decision Tree improved in recall, F1-score, and AUC after VAE augmentation.
- Random Forest achieved higher accuracy after augmentation.
- SVM showed a slight decrease in F1-score after augmentation.
- Naive Bayes remained mostly unchanged in accuracy, precision, recall, and F1-score.
- Overall, VAE augmentation did not consistently improve all classifiers.

This suggests that generative augmentation can influence model performance, but its effectiveness depends on the classifier and the quality of generated images.

## Important Result Files

The final results are stored in:

```text
results/model_a_vs_model_b_results.csv
results/model_b_improvement.csv
results/best_model_summary.csv
```

### `model_a_vs_model_b_results.csv`

Contains the full comparison between Model-A and Model-B for each classifier.

### `model_b_improvement.csv`

Shows the metric changes from Model-A to Model-B.

### `best_model_summary.csv`

Shows the best model for each evaluation metric.

## Generated Images

The VAE-generated images are stored in:

```text
augmented_images/benign/
augmented_images/malignant/
```

These images are synthetic and were used only for data augmentation experiments.

They should not be interpreted as real clinical mammogram images.

## How to Run the Notebook

1. Open the notebook in Google Colab.
2. Upload your Kaggle API file `kaggle.json`.
3. Run the dataset download section.
4. Run the preprocessing and image-path matching cells.
5. Run Model-A baseline training and evaluation.
6. Run the VAE augmentation section.
7. Run Model-B training and evaluation.
8. Run the Model-A vs Model-B comparison cells.
9. Export the results CSV files and generated images.

## Requirements

The notebook uses the following main Python libraries:

```text
numpy
pandas
matplotlib
opencv-python
scikit-learn
scikit-image
tensorflow
tqdm
kaggle
```

In Colab, most dependencies are already available. If needed, install missing libraries using:

```python
!pip install kaggle opencv-python scikit-image scikit-learn tensorflow tqdm
```

## Limitations

- VAE-generated images are often blurry and may not preserve all detailed mammogram structures.
- Traditional machine learning models have limited ability to learn complex spatial patterns compared with CNN-based models.
- HOG features capture edge and shape information but may not fully represent medical texture patterns.
- The red suspicious-region visualization is based on occlusion sensitivity and is not medical segmentation.
- The project is experimental and should not be used for clinical diagnosis.

## Future Work

Possible improvements include:

- Using CNN-based classifiers.
- Using pretrained deep learning feature extractors.
- Trying GAN-based or diffusion-based augmentation.
- Applying stronger image quality evaluation for generated images.
- Using texture features such as LBP or GLCM.
- Running cross-validation and hyperparameter optimization.
- Comparing traditional augmentation with generative augmentation.

## Project Deliverables

This repository includes:

- Code notebook for Model-A and Model-B.
- VAE-based data augmentation implementation.
- Generated synthetic images organized by class.
- Final results and analysis files.
- Presentation file.
- README documentation.

## Author

**Haitham Alawaji**  
King Saud University  
College of Computer and Information Sciences  
Information Technology Department  
Course: IT504 - Selected Topics

## Disclaimer

This project is for academic and experimental purposes only.  
The models and generated images are not intended for medical diagnosis or clinical decision-making.

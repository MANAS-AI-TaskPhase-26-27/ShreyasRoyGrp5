
### Training, Validation, and Test Sets

To train and evaluate a model effectively, a dataset is divided into three distinct partitions:

  

1. **Training Set (60–80%):** The primary portion of data used directly by the algorithm to learn internal patterns, weights, and parameters.
    
      
    
2. **Validation Set (10–20%):** Used during model development to tune hyperparameters (such as learning rate or tree depth) and compare different model architectures without introducing data leakage.
    
      
    
3. **Test Set (10–20%):** Held out until the end of model selection to evaluate final generalization performance on completely unseen real-world data.
    

### Machine Learning Models and Training Process

A **machine learning model** is a mathematical representation or function that transforms input features ($X$) into target outputs ($\hat{y}$).

  

The training process follows an iterative optimization loop:

  

1. **Forward Pass:** Input features are passed through the model to generate initial predictions.
    
      
    
2. **Loss Calculation:** A **loss function** quantifies the error between the model's predictions ($\hat{y}$) and actual targets ($y$).
    
      
    
3. **Optimization:** An optimization algorithm (such as **Gradient Descent**) updates internal model parameters (weights and biases) in the direction that minimizes the loss.
    
      
    
4. **Convergence:** Step 1–3 repeat across multiple training epochs until loss reaches a minimal threshold.
    
      
    

Training is an iterative feedback loop where optimization algorithms update model parameters to minimize prediction error.

## Data Preprocessing

### Importance of Data Cleaning

Raw real-world data is frequently noisy, incomplete, duplicate, or improperly formatted. Feeding raw data directly into algorithms leads to poor model performance—a principle known as _"Garbage In, Garbage Out"_. Data preprocessing cleanses and transforms raw inputs into a structured format suitable for mathematical modeling.

  

### Missing Data Handling

Missing values occur due to sensor errors, non-responses, or system glitches. Common strategies to handle missing values include:

- **Deletion:** Removing rows or columns containing missing entries (`df.dropna()`). Best reserved for cases where missingness is small (<5%) and random.
- **Mean Imputation:** Replacing missing entries with the feature's numerical average. Best suited for normally distributed data without severe outliers.
- **Median Imputation:** Replacing missing entries with the middle value of ordered data. Highly robust against skewed distributions and extreme outliers.
- **Mode Imputation:** Replacing missing entries with the most frequently occurring value, ideal for categorical variables.


 Use mean imputation for symmetrical numerical distributions, median imputation for skewed data with outliers, and mode imputation for categorical attributes.
### Outlier Treatment

**Outliers** are extreme data points that differ significantly from the rest of the observations. Outliers can heavily skew mean calculations, variance, and distance-based metrics.

- **Detection:** Identified via statistical methods like **Z-Score** (data points beyond $\pm 3$ standard deviations) or **Interquartile Range (IQR)** bounds.
- **Treatment Options:**
    - _Trimming:_ Removing genuine measurement errors or corrupted records.
    - _Winsorization (Capping):_ Bounding extreme values to set percentile thresholds (e.g., 1st and 99th percentiles).
    - _Log Transformation:_ Applying logarithmic functions to compress skewed scales.

Outliers should be analyzed carefully before removal, as they can represent valuable signals or domain-specific anomalies.
### Categorical Encoding

Machine learning models require numerical inputs for matrix multiplication and mathematical calculations. Categorical data must be converted into numeric formats:

- **One-Hot Encoding:** Creates binary columns (containing 0s and 1s) for each unique category. It is ideal for nominal categories with no inherent order (e.g., `City: [New York, London, Tokyo]`) to avoid imposing arbitrary mathematical hierarchy.

- **Label / Ordinal Encoding:** Assigns sequential integers (e.g., 0, 1, 2) to categories. Suitable for ordinal features with clear intrinsic rankings (e.g., `Education: [High School, Bachelor's, PhD]`).

Use One-Hot Encoding for nominal variables without rank, and Ordinal Encoding for categorical features with meaningful order.

### Feature Scaling

Features measured on vastly different scales (e.g., `Age` ranging 0–100 vs. `Income` ranging 10,000–1,000,000) can cause gradient-based optimization algorithms to converge slowly and distance-based models (e.g., K-Nearest Neighbors, SVMs) to bias toward high-magnitude features.

  

- **Standardization (Z-score Normalization):** Centers data around a mean of 0 with a standard deviation of 1.
$$x' = \frac{x - \mu}{\sigma}$$
- **Min-Max Normalization:** Rescales values linearly into a fixed range, typically $[0, 1]$.
 $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
 Feature scaling standardizes feature magnitudes, ensuring balanced feature contribution and efficient model training.

## Model Performance Concepts

### Overfitting vs. Underfitting

A central goal in machine learning is achieving strong generalization on unseen test data.

|**Problem**|**Description**|**Cause**|**Solution**|
|---|---|---|---|
|**Underfitting**|High training error and high test error (High Bias).|Model is too simple to learn underlying trends.|Increase model complexity, add features, reduce regularization.|
|**Overfitting**|Very low training error, but high test error (High Variance).|Model memorizes training noise instead of general patterns.|Use regularization, collect more data, simplify model, use dropout.|
|**Optimal Fit**|Low training error and low test error.|Balanced trade-off between bias and variance.|Maintain tuned cross-validation and feature selection.|

### Evaluation Metrics

Evaluating classification models strictly by Accuracy can be misleading, especially on imbalanced datasets (e.g., fraud detection where 99% of transactions are normal). Standard classification metrics include:



- **Accuracy:** The fraction of total predictions that were correct:    
$$\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$$
- **Precision:** The proportion of positive predictions that were actually correct (focuses on minimizing False Positives):
$$\text{Precision} = \frac{TP}{TP + FP}$$
- **Recall (Sensitivity):** The proportion of actual positive cases correctly identified (focuses on minimizing False Negatives):
$$\text{Recall} = \frac{TP}{TP + FN}$$

- **F1-Score:** The harmonic mean of Precision and Recall, providing a balanced single metric when class distributions are imbalanced:
  $$\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$
- **ROC-AUC:** Area Under the Receiver Operating Characteristic Curve. Measures a binary classifier's ability to discriminate between classes across all possible classification thresholds. An AUC of 1.0 represents perfect discrimination, while 0.5 indicates random guessing.

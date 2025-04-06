# Feedback-Analysis
# Multi-Label Classification using BERT

## 1. Understanding the Problem
The task is to classify items into multiple categories (Level 1 Factors). Since each item can belong to more than one category, this is a **multi-label classification** problem. The goal is to predict the categories based on the provided item descriptions.

---

## 2. Data Exploration
- **Training Data**: `bodywash-train.xlsx` contains:
  - `Core Item`: Text descriptions.
  - `Level 1 Factors`: Comma-separated category labels.
- **Test Data**: `bodywash-test.xlsx` contains:
  - `Core Item` column only. Predictions for this data will be made using the trained model.

---

## 3. Data Preprocessing
### Text Cleaning:
Performed the following preprocessing on item descriptions:
- Converted text to lowercase.
- Removed special characters, numbers, and punctuation.
- Removed stopwords using **NLTK**.
- Removed extra whitespace.

### Multi-Label Encoding:
- Used `MultiLabelBinarizer` to transform comma-separated labels into a one-hot encoded binary format.

---

## 4. Model Selection
Initially considered **Logistic Regression**, but due to low F1-score, switched to **BERT** (`bert-base-uncased`) for its:
- Contextual language understanding via attention mechanisms.
- Pre-trained embeddings.
- Easy adaptability for multi-label classification.

---

## 5. Model Training

### Tokenizer:
- Used BERT’s tokenizer to convert text to token IDs and attention masks.

### Custom Dataset:
- Created a custom `PyTorch Dataset` class to handle text and labels.

### Model Architecture:
- Used `BertForSequenceClassification` from HuggingFace Transformers.
- Modified `num_labels` based on the number of unique labels.
- Set `problem_type="multi_label_classification"`.

### Loss and Optimizer:
- **Loss**: `BCEWithLogitsLoss` for multi-label classification.
- **Optimizer**: `AdamW` with learning rate `1e-5`.

---

## 6. Training Loop
- Trained for **3 epochs** using `DataLoader` with batch size 16.
- Performed **backpropagation** using BCE loss.
- Printed training and validation loss at each epoch.

---

## 7. Prediction on Test Data
- Applied same tokenization steps on the test data.
- Generated predictions (logits), applied **sigmoid** to get probabilities.
- Used a **threshold of 0.5** to determine the final category predictions.
- Converted binary predictions back to labels using `inverse_transform` from `MultiLabelBinarizer`.

---

## 8. Saving Results
- Appended predicted labels to the test dataframe.
- Saved results to `bodywash_test_predictions.xlsx`.

---

## 9. Conclusion
This approach successfully leveraged the power of a pre-trained BERT model for multi-label classification. The attention-based contextual representation of BERT helps in understanding item descriptions effectively and makes accurate predictions of relevant categories.

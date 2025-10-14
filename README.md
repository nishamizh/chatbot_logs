# 🤖 chatbot_logs: Transformer-Based Log Level Classification

## 📊 Transformer-Based Log Level Classification Interactive Report

## 📘 Overview
This repository contains an interactive single-page application (SPA) that showcases a complete ML pipeline for automated log severity classification using Transformer models.
The core objective of this project is to leverage the power of Transformer models (specifically DistilBERT) to perform rapid, high-confidence severity analysis, significantly improving log monitoring and incident response workflows.

The report provides a complete narrative, from initial data exploration (EDA) through the machine learning pipeline, culminating in an interactive demonstration of the final classifier.

## 🚀 Key Features

- **Interactive Data Exploration**  
  Visualize log level distribution, temporal trends, and component-specific breakdowns using Chart.js.

- **Visual ML Pipeline**  
  Step-by-step breakdown of feature engineering, tokenization, model selection, and training.

- **Simulated Interactive Demo**  
  Input custom log details and receive real-time classification predictions (`WARN`, `INFO`, `ERROR`, `FATAL`).

- **Responsive Design**  
  Built with vanilla JavaScript, HTML, and Tailwind CSS for optimal viewing on all devices.


### 💻 Technical Details
| Category       | Library/Module                | Purpose                                                                 |
|----------------|-------------------------------|-------------------------------------------------------------------------|
| Deep Learning  | `PyTorch`, `transformers`     | Core framework and access to pre-trained Transformer models (DistilBERT). |
| Data Handling  | `Pandas`, `NumPy`             | Data manipulation, feature engineering, and numerical processing.       |
| ML Utilities   | `Scikit-learn` (`train_test_split`) | Splitting data into training and evaluation sets.                  |
| Datasets       | `Hugging Face Datasets`       | Efficient data loading and processing for Transformer training.         |
| Evaluation     | `Hugging Face Metrics`        | Calculating custom classification metrics (F1-score, Accuracy).         |



Model input is formed by combining Component, Content, and EventTemplate fields.

## 🧠 Model Pipeline Summary

- **Data Preparation & Feature Engineering**  
  Combine key log fields into a single input string:
  ```python
  input_text = log['Component'] + " | " + log['Content'] + " | " + log['EventTemplate']

Tokenization: The input text is converted into a numerical sequence using the pre-trained distilbert-base-uncased tokenizer.

Model Architecture: A sequence classification head is added atop the DistilBERT base model. The entire network is fine-tuned on the labeled log dataset.

### Evaluation: 
The model demonstrated strong performance on the held-out test set, achieving a final evaluation loss of 0.0098.


---

### 6. **Usage and Navigation**
Use bullet points and bold keywords:
```markdown
## 🛠 Usage and Navigation

- **Download**: Clone this repository or download the `index.html` file.
- **Open**: Launch `index.html` in any modern web browser.
- **Navigate**: Use the sticky navbar to jump between sections: Overview, Data Exploration, Model Pipeline, and Interactive Demo.
- **Interact**: In the Top Components Analysis chart, click any component bar to update the adjacent chart with its log level distribution.

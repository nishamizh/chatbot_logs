# 🤖 chatbot_logs: Transformer-Based Log Level Classification

## 📊 Transformer-Based Log Level Classification Interactive Report
### Overview
This repository hosts an interactive single-page application (SPA) that serves as a detailed report on building a machine learning system for automated infrastructure log level classification. The core objective of this project is to leverage the power of Transformer models (specifically DistilBERT) to perform rapid, high-confidence severity analysis, significantly improving log monitoring and incident response workflows.

The report provides a complete narrative, from initial data exploration (EDA) through the machine learning pipeline, culminating in an interactive demonstration of the final classifier.

### 🚀 Key Features
Interactive Data Exploration: Visualize log level distribution, temporal trends, and component-specific log breakdowns using dynamic Chart.js visualizations.

Visual ML Pipeline: A step-by-step breakdown of the feature engineering, tokenization, model selection, and training process.

Simulated Interactive Demo: A hands-on section where users can input custom log details and receive a simulated real-time classification prediction (WARN, INFO, ERROR, or FATAL).

Responsive Design: Built using vanilla JavaScript, HTML, and Tailwind CSS for optimal viewing on all devices (mobile and desktop).

### 💻 Technical Details
| Category       | Library/Module                | Purpose                                                                 |
|----------------|-------------------------------|-------------------------------------------------------------------------|
| Deep Learning  | `PyTorch`, `transformers`     | Core framework and access to pre-trained Transformer models (DistilBERT). |
| Data Handling  | `Pandas`, `NumPy`             | Data manipulation, feature engineering, and numerical processing.       |
| ML Utilities   | `Scikit-learn` (`train_test_split`) | Splitting data into training and evaluation sets.                  |
| Datasets       | `Hugging Face Datasets`       | Efficient data loading and processing for Transformer training.         |
| Evaluation     | `Hugging Face Metrics`        | Calculating custom classification metrics (F1-score, Accuracy).         |



Model input is formed by combining Component, Content, and EventTemplate fields.

### 🧠 Model Pipeline Summary
The classification system is built on a structured machine learning pipeline:

### Data Preparation & Feature Engineering:
Key log fields are combined into a single input string to provide maximal context:

input_text = log['Component'] + " | " + log['Content'] + " | " + log['EventTemplate']

Tokenization: The input text is converted into a numerical sequence using the pre-trained distilbert-base-uncased tokenizer.

Model Architecture: A sequence classification head is added atop the DistilBERT base model. The entire network is fine-tuned on the labeled log dataset.

### Evaluation: 
The model demonstrated strong performance on the held-out test set, achieving a final evaluation loss of 0.0098.

### 🛠 Usage and Navigation
The report is a self-contained HTML file (index.html) and requires no external server to run.

Download: Clone this repository or download the index.html file.

Open: Open index.html directly in any web browser.

Navigate: Use the sticky navigation bar at the top to jump between sections: Overview, Data Exploration, Model Pipeline, and Interactive Demo.

Interact: In the Top Components Analysis chart, click on any component bar to instantly update the adjacent chart, revealing the specific distribution of log levels for that component.

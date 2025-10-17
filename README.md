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


## 1. Problem Statement:
Infrastructure logs grow rapidly, non-technical support teams are increasingly tasked with assessing log severity from ticket content. Empowering them with a chatbot that interprets log severity reduces triage time for low-priority issues, updates ticket severity automatically, and engineers to focus on critical incidents.

### Context:
As infrastructure scales, the volume of system logs grows rapidly, making manual triage increasingly inefficient. Non-technical support teams receive tickets containing severity levels and raw logs, but these severity labels are often misaligned with the actual log content and summary. This leads to redundant triage efforts by engineers, reducing their productivity. By enabling support teams to validate and update ticket severity based on log analysis, we can streamline triage and allow engineers to focus on high-impact tasks.

### Solution Overview:
Empowering them with a chatbot that interprets log severity reduces triage time for low-priority issues, updates ticket severity automatically, and engineers to focus on critical incidents.

### Product Outcome:
The top Severity tasks are  prioritized and are completed on time before it creates a bigger issue.

## Key data sources:
The Data used for training are 2 Csv file that has logs for various types of Logs for Hadoop and Zookeeper.
Model input is formed by combining Component, Content, and EventTemplate fields.

## 2. Data Wrangling:
####  <i> About the Data </i> 
This particular data has 4000 rows.
The Hadoop and Zookeeper Data logs had the columns 'LineId', 'Date', 'Time', 'Level', 'Component', 'Content', 'EventId', 'EventTemplate', 'LogType' in common. The Hadoop has an extra column 'Process' whereas the Zookeeper has an extra column 'Node'. 
####  <i>  For this Data wrangling the steps I followed:  </i> 
These additional columns are renamed as AdditionalInformation which has the Dictionary value to have the <column_name> : value as their data in String format for both the Zookeper and Data.
During this phase the extra columns is <b>LogType</b> is added to have a constant value as "Hadoop" for Hadoop data or "Zookeeper" for Zookeeper data is added as an extra feature column.
The merging of these Hadoop and Zookeeper Data Logs are merged as a single integrated Log - data_logs and they are used in the future. 
The Data is checked for any missing values. Date and Time Columns are merged and set to have type Date.
The EventTemplate column is removes extra spaces (2 or more consecutive spaces) from each string and they are replaced with <VAR> and all the regexp clean up are done.
Normalization of the Column - Level (Target) is done to gain insight about the balance in the data.
An additional column - <b>input</b> is made as dictionary in the format <column_name> : value for the Timestamp, LogType, Columns Component, Content, EventId, EventTemplate.

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

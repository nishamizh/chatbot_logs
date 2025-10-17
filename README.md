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
####  <i> 🗂️ About the Data </i>
The dataset contains 4,000 rows of log entries from Hadoop and Zookeeper systems. Both share common columns: LineId, Date, Time, Level, Component, Content, EventId, EventTemplate, and LogType. Hadoop includes an extra column Process, while Zookeeper includes Node.

####  <i>  🔧 Wrangling Steps:  </i> 
Unified schema: The extra columns (Process and Node) are renamed to AdditionalInformation, stored as stringified dictionaries with <column_name>: value format.

Source tagging: A new column LogType is added to label each entry as either "Hadoop" or "Zookeeper".

Data integration: Both logs are merged into a single DataFrame called data_logs.

Missing values: Checked and handled appropriately.

Timestamp creation: Date and Time columns are merged and converted to datetime format.

Template cleanup: EventTemplate strings are cleaned using regex—extra spaces are removed and replaced with <VAR>.

Target normalization: The Level column is normalized to assess class balance.

Input formatting: A new column input is created as a dictionary containing Timestamp, LogType, Component, Content, EventId, and EventTemplate.

Tokenization: These inputs are converted to numerical format using the pretrained DistilBERT tokenizer.

## 3. EDA
📊 Visual Insights
Log Level Distribution: Bar plot showing frequency of each log level.

Temporal Trends: Stacked bar chart visualizing log levels over time.

Frequent Errors: Top 20 components with the most error logs, visualized alongside EventTemplate frequency.

Token Count Analysis: Histogram of token counts per log entry—over 80% have fewer than 20 tokens.

Component-Level Patterns: Grouped analysis of Component and Level to identify recurring combinations.

Log Rate: Line plot showing number of logs per minute.

Token Length by Level: Average token count per log level.

Normalized Component-Level Distribution: Stacked bar chart showing percentage breakdown of log levels within top components.

Label Encoding of the Log LEVEL is done.
target   Level
0        ERROR
1        FATAL
2        INFO
3        WARN

Export the final cleaned data to support both exploratory analysis and machine learning modeling.

## 4. Data Pre Processing
X = data['input']
y = data['target']

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

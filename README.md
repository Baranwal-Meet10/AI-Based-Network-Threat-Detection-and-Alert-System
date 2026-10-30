🛡️ AI-Based Network Threat Detection and Alert System

An academic Computer Networks mini project that explores AI/ML-based network intrusion detection using the **NSL-KDD dataset**. The project preprocesses network-traffic records, compares a **Random Forest** classifier with a **Deep Neural Network (DNN)**, classifies traffic into normal and attack categories, and generates alerts for detected suspicious activity.

> **Project domain:** Network Security · Intrusion Detection · Machine Learning · Deep Learning

## 📌 Project Overview

Network Intrusion Detection Systems (NIDS) monitor network traffic to identify potentially malicious activity. Traditional signature-based approaches can struggle with previously unseen attacks, while anomaly-based approaches may produce false alarms.

This project studies and implements a machine-learning-based approach to network threat detection. It uses the NSL-KDD dataset to train and evaluate two models:

- **Random Forest** — a machine-learning classifier.
- **Deep Neural Network (DNN)** — a deep-learning classifier.

The system also demonstrates alert generation and traffic visualisation using test data.

## 🎯 Objectives

- Understand network intrusion detection and its role in network security.
- Prepare and preprocess network-traffic data for model training.
- Study and compare machine-learning and deep-learning approaches.
- Classify traffic into normal and attack categories.
- Evaluate models using accuracy, precision, recall, F-measure, false alarm rate, and detection rate.
- Generate alerts for detected suspicious traffic and save detection outputs.

## ✨ Features

- **NSL-KDD dataset processing:** Loads and prepares network-traffic records for analysis.
- **Data preprocessing:** Handles duplicate and missing records, encodes categorical features, and scales numerical features.
- **Model comparison:** Evaluates Random Forest and Deep Neural Network models.
- **Multi-class classification:** Works with the NSL-KDD traffic categories: DoS, Normal, Probe, R2L, and U2R.
- **Alert generation:** Displays alerts for detected attack traffic, with severity labels shown in the project output.
- **Visualisation:** Displays model comparisons, confusion matrices, and detected traffic distributions.
- **CSV outputs:** Saves detection results and alert logs.

## 🧠 System Workflow

```text
             NSL-KDD Dataset
                    |
                    v
             Data Loading
                    |
                    v
              Preprocessing
       (cleaning, encoding, scaling)
                    |
                    v
          Feature Preparation
                    |
                    v
          Model Training / Testing
              /           \
             v             v
       Random Forest      DNN
              \           /
               v         v
             Evaluation
                    |
                    v
          Traffic Classification
          (Normal / Attack Classes)
                    |
                    v
             Alert Generation
                    |
                    v
          Results and Visualisation
```

## 🗂️ Dataset

This project uses the **NSL-KDD** network intrusion detection dataset.

The dataset contains network-connection records with 41 traffic features. Its attack categories are grouped into:

| Category | Description |
|---|---|
| Normal | Legitimate network activity |
| DoS | Denial-of-Service attacks |
| Probe | Scanning and information-gathering activity |
| R2L | Remote-to-Local attacks |
| U2R | User-to-Root privilege-escalation attacks |

The report describes using `KDDTrain+.txt` and `KDDTest+.txt` for dataset loading and identifies `test_prob.csv` as the testing input used for the final detection demonstration.

**Dataset note:** Obtain NSL-KDD from its official or recognised distribution source. The dataset files are not included in this repository unless explicitly added by the project team.

## 🛠️ Technologies and Libraries

The report's implementation screenshots show the use of the following tools and libraries:

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| pandas | Dataset loading and manipulation |
| NumPy | Numerical operations |
| scikit-learn | Preprocessing, Random Forest, and evaluation |
| imbalanced-learn | Class-imbalance handling utilities |
| TensorFlow / Keras | Deep Neural Network implementation |
| Matplotlib | Charts and result visualisation |
| Seaborn | Data visualisation |
| Jupyter Notebook / Google Colab | Development and experimentation environment |

## 📊 Model Evaluation

The report compares Random Forest and Deep Neural Network results using the test data. The values below are transcribed from the model-comparison output shown in the project report.

| Metric | Random Forest | Deep Neural Network |
|---|---:|---:|
| Accuracy | 0.7735 | 0.7887 |
| Precision | 0.8266 | 0.8250 |
| Recall | 0.7735 | 0.7887 |
| F-measure | 0.7419 | 0.7633 |
| False Alarm Rate (FAR) | 0.0257 | 0.0298 |
| Detection Rate | 0.6436 | 0.6731 |
| Training Time (seconds) | 136.1 | 43.8 |

**Note:** These are the values displayed in the report's model-comparison output. Results may vary with dataset preparation, train/test split, model configuration, and execution environment. Accuracy alone does not describe performance across every attack class; per-class results and the confusion matrices should also be considered.

## 🚨 Alert and Output Files

The detection demonstration in the report shows generated alerts for traffic classified as suspicious, including Probe and DoS examples. The output section also shows traffic-class charts and a traffic-share visualisation.

The run shown in the report saves:

- `detection_results.csv` — detection output records.
- `alerts_log.csv` — generated alert records.

The report's final testing demonstration uses `test_prob.csv` as the testing input.

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/AI_Based_Network_Threat_Detection_and_Alert_System.git
cd AI_Based_Network_Threat_Detection_and_Alert_System
```

### 2. Create and activate a virtual environment

**Windows**

```bash
python -m venv venv
venv\Scripts\activate
```

**Linux / macOS**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

If the repository contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

The report screenshots show these Python packages/imports being used:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn tensorflow
```

### 4. Add the dataset

Download the NSL-KDD dataset and place the required files in the dataset location expected by the notebook or source code. The report shows `KDDTrain+.txt` and `KDDTest+.txt` being used.

### 5. Run the project

Open the project's notebook in **Jupyter Notebook** or **Google Colab** and execute the cells in order. Use the actual notebook or script included in this repository; the exact execution filename should be documented here once the project files are finalised.

## 👥 Project Team

Developed as a Computer Networks (CN) mini project at **Unitedworld Institute of Technology, Karnavati University**.

| Team Member |
|---|
| Meet Baranwal |
| Rohit Maheshwari |
| Hawan Sharma |
| Prarthan Christi |

**Project Mentor:** Ms. Pragya Vats

**Programme:** B.Tech — Computer Science Engineering

## 🔮 Future Scope

- Evaluate the models on additional and more recent intrusion-detection datasets.
- Improve detection of minority attack classes such as R2L and U2R.
- Explore feature selection and dimensionality reduction to reduce model complexity.
- Investigate hybrid ML/DL approaches.
- Extend the system towards real-time network traffic monitoring.
- Test the approach with live or realistic network traffic.
- Explore periodic model updates as new traffic patterns emerge.

## 📚 Reference

Z. Ahmad, A. S. Khan, C. W. Shiang, J. Abdullah, and F. Ahmad, “Network intrusion detection system: A systematic study of machine learning and deep learning approaches,” *Transactions on Emerging Telecommunications Technologies*, 2020, Article e4150. https://doi.org/10.1002/ett.4150

## ⚠️ Disclaimer

This project is developed for academic and educational purposes. It demonstrates network intrusion detection using the dataset and experimental setup described in the project report. It should not be treated as a production-ready network security solution.

---

**Made with Python, Machine Learning, and a focus on Network Security.** 🔐


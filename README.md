# AI-Based Network Threat Detection and Alert System

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Latest-green.svg)](https://scikit-learn.org/)
[![Dataset](https://img.shields.io/badge/Dataset-NSL--KDD-yellow.svg)](https://www.unb.ca/cic/datasets/nsl.html)
[![Dashboard](https://img.shields.io/badge/UI-SOC%20Console-blueviolet.svg)](dashboard/index.html)
[![Course](https://img.shields.io/badge/Academic-Computer%20Networks%20Mini--Project-lightgrey.svg)]()

> An end-to-end network intrusion detection and automated alert triage system powered by Machine Learning (Random Forest) and Deep Learning (Multi-Layer Perceptron), trained on the benchmark NSL-KDD dataset. Includes an interactive, zero-dependency, single-file SOC (Security Operations Center) dashboard.

---

## Academic Context & Team

**Course:** Computer Networks (B.Tech Computer Science & Engineering)  
**Institution:** Unitedworld Institute of Technology / Karnavati University  
**Academic Period:** July – November 2026  
**Faculty Mentor:** Ms. Pragya Vats  

**Project Contributors:**
- **Meet Baranwal** — *KU2407U726*
- **Rohit Maheshwari** — *KU2407U449*
- **Hawan Sharma** — *KU2407U071*
- **Prarthan Christi** — *KU2407U167*

---

## Executive Summary

Traditional network security appliances (legacy stateless firewalls and signature-based NIDS) struggle against novel polymorphic threats, zero-day exploits, and distributed evasion maneuvers. This project demonstrates an intelligent, anomaly-driven Network Intrusion Detection System (NIDS) that:

1. **Ingests Connection Records:** Parses 41-feature TCP/IP network connection flows.
2. **Mitigates Extreme Class Imbalance:** Employs Synthetic Minority Over-sampling Technique (**SMOTE**) exclusively on rare attack classes in training data.
3. **Optimizes Latency & Collinearity:** Projects high-dimensional one-hot encoded features (122 dimensions) down to 28 orthogonal components via **Principal Component Analysis (PCA)**, preserving 95% of data variance.
4. **Benchmarks Multi-Class Classifiers:** Compares an ensemble **Random Forest (100 estimators)** against a **Deep Neural Network (MLP with Dropout regularization)**.
5. **Automates Incident Triaging:** Generates actionable, severity-rated security alerts (`CRITICAL`, `HIGH`, `MEDIUM`, `NONE`) and exports incident audit logs.
6. **Delivers Real-Time Visualization:** Features a standalone, performant web SOC console that replays connection streams with rolling threat share analytics and confusion matrix inspection.
---

## Attack Taxonomy & Threat Severity Matrix

Network traffic flows are classified into five operational classes (one benign class and four threat categories):

| Attack Class | Severity Level | Threat Profile & Operational Impact | Representative Attacks in Dataset |
|:---|:---:|:---|:---|
| **Normal** | `NONE` | Legitimate user sessions and valid protocol exchanges. | Valid HTTP browsing, SMTP mail transfers, DNS lookups |
| **DoS** (Denial of Service) | `HIGH` | Floods network links or host system resources to induce latency or service crashes. | `neptune`, `smurf`, `back`, `teardrop`, `pod`, `land` |
| **Probe** | `MEDIUM` | Reconnaissance scanning of subnet ranges, ports, and services prior to exploitation. | `portsweep`, `nmap`, `satan`, `ipsweep` |
| **R2L** (Remote to Local) | `HIGH` | Unauthorized access gained by a remote attacker without local credentials. | `guess_passwd`, `ftp_write`, `imap`, `phf`, `warezclient` |
| **U2R** (User to Root) | `CRITICAL` | Privilege escalation where an authenticated standard user elevates to superuser/root. | `buffer_overflow`, `loadmodule`, `rootkit`, `perl` |

---

## Dataset Description & Preprocessing

The model is trained and evaluated on **NSL-KDD**, the authoritative benchmark solving duplicate-record skew in the legacy KDD Cup 99 corpus:

- **Training Records (`KDDTrain+`):** 125,964 connection records.
- **Test Records (`KDDTest+`):** 22,541 connection records (includes 17 zero-day/novel attack variants absent from training sets to simulate realistic evasion).
- **Feature Vector (41 Raw Attributes):**
  - *Basic Features (1–9):* Duration, protocol type (`tcp`, `udp`, `icmp`), service (`http`, `private`, `ftp`, etc.), connection flags (`SF`, `REJ`, `S0`, etc.), source/destination bytes.
  - *Content Features (10–22):* Failed login count, logged-in status, compromised state, root shell access.
  - *Time-based Traffic Features (23–31):* Number of connections to the same host in a 2-second sliding window, SYN/REJ error rates.
  - *Host-based Traffic Features (32–41):* Cumulative connection counts across a 100-connection window, service distribution variance.

### Data Engineering Steps:
1. **Categorical Encoding:** One-hot encoding applied to `protocol_type`, `service`, and `flag` expands the feature space from 41 to 122 dimensions.
2. **Numeric Normalization:** Min-Max scaling normalizes continuous variables between $[0, 1]$ to stabilize neural network gradient descent.
3. **SMOTE Balancing:** Training-split minority classes (R2L: 995, U2R: 52) are synthetically oversampled to 5,000 samples each using 5-nearest neighbors. **The test set is left completely un-resampled to maintain real-world validation integrity.**
4. **PCA Transformation:** Preserving 95% of spectral variance compresses the 122 features into **28 principal components**, reducing training time by over 60% with negligible loss in fidelity.

---

## Model Benchmark Results

Evaluated on the full un-resampled **KDDTest+** distribution (22,541 connections):

| Performance Metric | Random Forest (100 Trees) | Deep Neural Network (MLP) | Optimal Model |
|:---|:---:|:---:|:---:|
| **Accuracy** | 77.35% | **78.87%** | **DNN (+1.52%)** |
| **Precision (Weighted)** | **82.66%** | 82.50% | **RF (+0.16%)** |
| **Recall (Weighted)** | 77.35% | **78.87%** | **DNN (+1.52%)** |
| **F-Measure / F1 Score** | 74.19% | **76.33%** | **DNN (+2.14%)** |
| **False Alarm Rate (FAR)** | **2.57%** | 2.98% | **RF (-0.41%)** |
| **Detection Rate (DR)** | 64.36% | **67.31%** | **DNN (+2.95%)** |
| **Training Duration** | 136 seconds | **44 seconds** | **DNN (3.1× faster)** |

> *Note:* Exact metrics may exhibit minor stochastic variation across different hardware and random initialization seeds.

### Per-Class Detection Analysis & Limitations:
- **DoS Catch Rate:** Excellent performance (Precision: 96.5%, Recall: 80.1%). Signature traffic volumes are recognized cleanly.
- **Probe Detection:** Strong performance (Precision: 86.8%, Recall: 64.8%).
- **R2L & U2R Limitations (Real-World Analysis):** R2L recall is ~11.8% and U2R recall is ~17.9%. Despite SMOTE augmentation, these attacks execute single-packet stealth exploits that look identical to normal administrative traffic. Furthermore, the test split contains novel exploit payload variants not present in the training set. This mirrors the findings documented in published literature (Ahmad et al., 2020).

---

## Interactive SOC Monitoring Console

Located at [`dashboard/index.html`](dashboard/index.html), the project features an optimized single-file operations dashboard:

- **Live Monitor (`#monitor`):** Real-time deterministic connection stream player with drift-compensated scheduling, automated Page Visibility tab pausing, rolling 20-connection threat share SVG chart, live alert feed with entrance animations, and detection meters per attack family.
- **Model Performance (`#performance`):** Interactive confusion matrix (22,541 connections) with color-mix heatmapping, weighted KPI metric cards, and per-class precision/recall/F1 breakdowns.
- **Data & Pipeline (`#pipeline`):** Visual step-by-step pipeline walkthrough, SMOTE balance comparison (before vs after), protocol distributions, and top targeted network services.
- **Engineering Quality:** Zero external frameworks or build steps, accessible roving tabindex keyboard navigation (`Arrow`, `Home`, `End`), WCAG-compliant live regions (`aria-live`), and dark/light system theme support.

---

## Repository Structure

```text
AI-Based-Network-Threat-Detection-and-Alert-System/
├── AI_Based_Network_Threat_Detection_and_Alert_System.ipynb  # Primary Jupyter/Colab notebook
├── README.md                                                 # Technical documentation & project guide
├── requirements.txt                                          # Python runtime dependencies
├── dashboard/
│   └── index.html                                            # High-performance, single-file SOC dashboard
└── sample_test_files/                                        # Ready-to-use validation traffic files
    ├── test_dos.csv                                          # Isolated DoS attack samples
    ├── test_mixed.csv                                        # Unlabeled mixed multi-class traffic
    ├── test_mixed_labeled.csv                                # Ground-truth labeled verification sample
    ├── test_probe.csv                                        # Isolated Probe reconnaissance samples
    ├── test_r2l.csv                                          # Remote-to-Local exploit traffic
    └── test_u2r.csv                                          # Privilege escalation traffic
```

---

## Getting Started

### Option 1: Running in Google Colab (Recommended)
1. Open [Google Colab](https://colab.research.google.com).
2. Click **File -> Upload notebook** and select `AI_Based_Network_Threat_Detection_and_Alert_System.ipynb`.
3. Run all cells (**Runtime -> Run all**). The notebook automatically downloads and verifies the NSL-KDD dataset.
4. When execution reaches **Section 12 (Test Your Own File)**, click **Choose files** and upload any file from `sample_test_files/` (e.g., `test_mixed.csv`).
5. Review the generated alert summary, visualized charts, and download `detection_results.csv`.

### Option 2: Local Installation & Execution
```bash
# 1. Clone the repository
git clone https://github.com/<username>/AI-Based-Network-Threat-Detection-and-Alert-System.git
cd AI-Based-Network-Threat-Detection-and-Alert-System

# 2. Create and activate a virtual environment (recommended)
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

# 3. Install required libraries
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook AI_Based_Network_Threat_Detection_and_Alert_System.ipynb
```

### Option 3: Viewing the SOC Dashboard
Simply double-click [`dashboard/index.html`](dashboard/index.html) to open it in any modern browser (Chrome, Edge, Firefox, Safari). It operates completely offline with zero server requirements.

---

## Future Roadmap

- [ ] **Modern Datasets:** Benchmark across contemporary threat distributions (CIC-IDS2018, UNSW-NB15, TON_IoT).
- [ ] **Packet-Level Ingestion:** Implement a live packet sniffing layer (`scapy` / `zeek` / `pcap`) converting raw flows into feature vectors on-the-fly.
- [ ] **Transformer-based Anomaly Models:** Evaluate Temporal Convolutional Networks (TCN) or Tabular Transformers for long-range session context.
- [ ] **SIEM & Webhook Integration:** Add automated forwarding of `CRITICAL` alerts to Slack, Discord, or Splunk HTTP Event Collectors (HEC).

---

## References

1. Z. Ahmad, A. S. Khan, C. W. Shiang, J. Abdullah, and F. Ahmad, *"Network intrusion detection system: A systematic study of machine learning and deep learning approaches,"* Transactions on Emerging Telecommunications Technologies, vol. 32, no. 1, p. e4150, 2020. [DOI: 10.1002/ett.4150](https://doi.org/10.1002/ett.4150).
---

## License

This project is released for academic, research, and educational purposes under the [MIT License](LICENSE) (or Course Mini-Project Guidelines).

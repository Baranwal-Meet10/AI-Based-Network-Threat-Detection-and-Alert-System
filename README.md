# AI-Based Network Threat Detection and Alert System

Mini project for the Computer Networks course, B.Tech CSE, UWSB / Karnavati University (July - November 2026).

**Team:** Meet Baranwal (KU2407U726), Rohit Maheshwari (KU2407U449), Hawan Sharma (KU2407U071), Prarthan Christi (KU2407U167)
**Mentor:** Ms. Pragya Vats

## What this project is about

Normal firewalls and old-style intrusion detection systems only catch attacks they already know about. So we tried a different way: train a model on real network traffic records and let it learn what each type of attack looks like.

In this project you give the system a file of network connections. It looks at every connection and tells you whether it is normal traffic or an attack, and if it is an attack, which type. It then prints an alert with a severity level and saves everything in a results file.

We built two models and compared them:
- Random Forest (machine learning)
- Deep Neural Network (deep learning)

## Dataset

We used **NSL-KDD**. It is a cleaned-up version of the old KDD Cup 99 dataset, with duplicate records removed. Every row is one network connection described by 41 features (protocol, service, bytes sent, error rates and so on).

We grouped the attacks into 4 types:

| Type | What it means |
|---|---|
| DoS | Flooding a server so nobody else can use it |
| Probe | Scanning the network to find open ports and hosts |
| R2L | Remote to Local, an outsider trying to get access (for example guessing a password) |
| U2R | User to Root, a normal user trying to become admin |

Anything else is labelled Normal. The notebook downloads the dataset by itself. If the download fails it asks you to upload `KDDTrain+.txt` and `KDDTest+.txt`.

## How it works

1. Load NSL-KDD and group the attack names into the 4 types
2. Clean the data (remove duplicates and missing values), one-hot encode the text columns, scale the numbers
3. Fix the class imbalance with SMOTE (R2L and U2R have very few examples), only on the training data
4. Reduce the features with PCA (122 features down to 28, keeping 95% of the information)
5. Train Random Forest and the DNN
6. Compare them using accuracy, precision, recall, F-measure, detection rate and false alarm rate
7. Use the better model for the alert system, where you upload a traffic file and get the attack type

## How to run it

1. Open `NIDS_Threat_Detection.ipynb` in [Google Colab](https://colab.research.google.com) (File -> Upload notebook)
2. Click Runtime -> Run all. Training takes some minutes.
3. When it reaches the upload cell (Section 12) click "Choose files" and upload any file from the `sample_test_files` folder, for example `test_mixed.csv`
4. You will get the number of attacks of each type, the main threat, the alerts, charts, and a downloadable `detection_results.csv`

If you have a file with a `label` column (like `test_mixed_labeled.csv`) it also shows the accuracy for that file.

To run it on your own computer: `pip install -r requirements.txt` and open the notebook in Jupyter.

## Results

Tested on the official KDDTest+ file (22,541 connections after removing duplicates).

| Model | Accuracy | Precision | Recall | F-measure | False alarm rate | Detection rate | Training time |
|---|---|---|---|---|---|---|---|
| Random Forest | 77.35% | 82.66% | 77.35% | 74.19% | 2.57% | 64.36% | 136 s |
| Deep Neural Network | 78.87% | 82.50% | 78.87% | 76.33% | 2.98% | 67.31% | 44 s |

The DNN did slightly better and trained faster, so the alert system uses it.

These numbers can change a little each time you run the notebook, so your own run may not match exactly.

## Things that did not work so well

We want to be honest about this part:

- DoS is detected well (about 97% precision, 80% recall)
- R2L and U2R are detected badly. Out of 2885 R2L attacks the Random Forest found only about 340. This is because there are very few of them in the training data (only 52 U2R rows), and the test file also has attack variants the model never saw during training. This is a known problem with NSL-KDD and the same problem is mentioned in the paper we based our report on.
- NSL-KDD is an old dataset (2009), so the results may not match what happens on today's traffic
- The input file must be in NSL-KDD format (the 41 features). Raw packet captures (.pcap) will not work without first converting them to these features

## What is in this repository

```
NIDS_Threat_Detection.ipynb   the main notebook (training + detection)
sample_test_files/            small CSV files you can upload to test it
dashboard/index.html          a visual dashboard of the results
requirements.txt              python libraries needed
```

The dashboard is a standalone page that replays some test connections with alerts and shows the model results. You can open it in a browser. If GitHub Pages is switched on for this repository it is also available at `https://<username>.github.io/<repo-name>/dashboard/`.

## Future work

- Try newer datasets like CIC-IDS2018 or UNSW-NB15
- Improve R2L and U2R detection
- Test on live traffic instead of a saved dataset
- Retrain regularly so it can handle new attacks

## Reference

Z. Ahmad, A. S. Khan, C. W. Shiang, J. Abdullah, and F. Ahmad, "Network intrusion detection system: A systematic study of machine learning and deep learning approaches," Trans. Emerg. Telecommun. Technol., 2020, e4150. doi: 10.1002/ett.4150.

Dataset: M. Tavallaee et al., "A detailed analysis of the KDD CUP 99 data set", 2009 (NSL-KDD).

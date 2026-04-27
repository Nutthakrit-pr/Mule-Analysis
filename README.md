# Mule Account Detection & Network Analysis

## 📌 Project Overview
This project focuses on identifying and mitigating financial fraud—specifically **Mule Accounts (บัญชีม้า)**—by transitioning from traditional, high-latency rule-based systems to advanced behavioral and network-based detection. By analyzing transaction patterns, holding periods, and device-sharing behaviors, this project aims to stop fraudsters before funds can be successfully siphoned out.

## ⚠️ Problem Statement
Traditional rule-based detection systems suffer from high latency, allowing malicious actors to withdraw or transfer funds before banks can take action. These legacy systems also produce high false-positive rates and struggle to adapt to constantly evolving fraud patterns. 

## 🎯 SMART Objectives
* **Specific:** Develop a framework (including Graph Neural Network / GNN concepts) to identify mule account networks by treating accounts/devices as *Nodes* and transactions as *Edges*.
* **Measurable:** Reduce fraud-related financial losses by 15% and increase alert precision by 20%.
* **Achievable:** Utilize internal transaction logs, device IDs, and account age data to build real-time alerts.

---

## 📊 Dataset Description

The repository contains two processed datasets used for exploratory data analysis (EDA) and model building:

### 1. `cleaned_mule_data.csv`
This dataset powers the majority of the behavioral visualizations (e.g., Hit & Run patterns, device farms).
* `transaction_id`: Unique identifier for the transaction.
* `sender_age_days`: Age of the sender's account.
* `receiver_age_days`: Age of the receiver's account.
* `tx_amount_normalized`: Min-Max normalized transfer amount (scaled 0-1 to reduce bias from extreme outliers).
* `time_to_transfer_out_mins`: The holding period (time elapsed before funds are transferred out).
* `shared_device_count`: Number of accounts sharing the same Device ID or IP Address.
* `is_mule`: Target label (1 = Mule Account, 0 = Normal Account).

### 2. `correlation_heatmap.csv`
This dataset specifically powers the correlation matrix visualization, revealing the statistical relationships between different numerical features and the `is_mule` flag.

---

## 📈 Interactive Visualizations (Tableau)
### `Midterm_deck.twbx`
This repository includes a Tableau Packaged Workbook (`Midterm_deck.twbx`) that contains interactive dashboards for all the tables and datasets mentioned above. You can open this file using Tableau Desktop or Tableau Reader to dynamically explore the correlation matrices, device farm clusters, and holding-period distributions.

---

## 🔍 Key Insights & Findings 

All graphical visualizations generated in this project (as seen in `image_a0c398.png` and the Tableau dashboard) are directly aligned with the business hypotheses outlined in our canvas (`latest_canvas.pptx`).

### Insight 1: The "Hit & Run" Behavior (Short Holding Period)
* **Finding:** The data shows a heavy concentration of fraudulent activity among "0-day age" accounts (accounts used immediately upon opening). 
* **Pattern:** These accounts exhibit extremely short holding periods, transferring funds out within mere minutes. This confirms that legacy rule-based systems with high latency cannot react fast enough.

### Insight 2: "Device Farm" Network Vulnerabilities
* **Finding:** Shared devices are the strongest indicator of organized mule networks. 
* **Pattern:** Based on the correlation analysis, device sharing heavily influences the likelihood of fraud (Correlation +0.82). A clear statistical tipping point exists: **If 3 or more accounts share the same Device ID or IP Address, the probability of them being a mule network approaches 100%.** Individual monitoring systems typically miss these coordinated network patterns.

---

## 🛡️ Proposed Rule-Based Action Plan
Based on the data analysis, the following real-time rules are proposed to bridge the gap before a full GNN deployment:

1. **Rule-based Alert 1 (Device Farm):** Immediately freeze transactions when 3 or more accounts are detected sharing the same Device ID / IP address.
2. **Rule-based Alert 2 (Hit & Run on Aged Accounts):** Flag and alert on "aged accounts" that exhibit a sudden change in device followed by rapid, uncharacteristic transfers.

---

## 📂 Repository Structure
```text
├── data/
│   ├── cleaned_mule_data.csv       # Main dataset for behavioral graphs
│   └── correlation_heatmap.csv     # Data for the feature correlation heatmap
├── dashboards/
│   └── Midterm_deck.twbx           # Interactive Tableau dashboard visualizing all tables
├── docs/
│   └── latest_canvas.pptx          # Project canvas, hypotheses, and business requirements
├── images/
│   └── image_a0c398.png            # Static visualizations mapping data to business findings
└── README.md                       # Project overview and insights

# Manufacturing Spend Analytics & Audit Pipeline (v7)

## 📌 Overview
This repository contains a production-ready data pipeline designed to transform raw procurement and invoice data into audit-ready financial insights. The tool automates the cleaning of high-volume transaction records (36,000+ rows), handles multi-currency normalization, and applies machine learning to identify procurement anomalies.

## 🚀 Key Features
* **Currency Normalization Engine:** Automatically maps diverse transaction currencies (USD, JPY, SGD) to a base functional currency (**MYR**) using vectorized exchange rate mapping.
* **Data Masking & Privacy:** A custom hashing/masking layer for `Company` and `Vendor` identifiers to comply with internal corporate data privacy standards.
* **Audit-Ready Logic:**
  * **Benford’s Law Analysis:** Extracts the `First_Digit` of transaction amounts to identify potential manual entry manipulation.
  * **"Cliff Effect" Resolution:** Automatically trims incomplete end-of-period data to ensure trend accuracy.
  * **Vague Description Flagging:** Uses Regex to catch invoices with insufficient detail (e.g., "General," "Misc").
* **Anomaly Detection:** Integrated `Isolation Forest` (Scikit-Learn) to flag statistically significant outliers for internal audit review.

## 🎥 Visual Demonstration
![Pipeline Workflow & Analytics](images/demo.gif)
*The GIF above demonstrates the pipeline processing 36,000+ records and generating automated spend distribution charts.*

## 🛠️ Technical Workflow
1. **Ingestion:** Standardizes headers and performs safe type-conversion for Dates and Amounts.
2. **Feature Engineering:** Generates categorical flags like `Is_Q4` (to track year-end spending) and `Smart Audit Flags`.
3. **Visualization:** Uses Seaborn and Matplotlib to visualize spend by department (e.g., HR, Training, Operations).
4. **Anomaly Scoring:** Assigns a risk category (Normal vs. Outlier) based on transaction behavior.

## 📦 Requirements & Installation
To run this pipeline locally, you will need a CSV named `pi_test.csv` (schema: Date, Amount, Currency, Company, Description).

```bash
# Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn

# Run the notebook
jupyter notebook manu-v7.ipynb
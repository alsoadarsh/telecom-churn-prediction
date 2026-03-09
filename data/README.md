# Data

## Source

IBM Telco Customer Churn dataset, available on Kaggle:  
https://www.kaggle.com/datasets/blastchar/telco-customer-churn

Download `WA_Fn-UseC_-Telco-Customer-Churn.csv` and place it in this folder.

## How the Data Was Split

The original dataset (7,043 rows) was divided into two subsets for this project:

- **Model Building** (80% — 6,499 rows): Used to train and evaluate all five models
- **Implementation** (remaining rows): Held out to simulate real deployment — 
  the trained model scores these 500 customers and produces the revenue figures 
  in the analysis

To recreate the split and generate the `.rda` files the Rmd expects, 
run this in R from the `data/` folder:
```r
library(tidyverse)

# Load raw data
df <- read_csv("WA_Fn-UseC_-Telco-Customer-Churn.csv")

# Rename churn column to match analysis
df <- df %>% rename(Status = Churn) %>%
  mutate(Status = ifelse(Status == "Yes", "Left", "Current"))

# Split
set.seed(42)
idx <- sample(nrow(df), 6499)

Model_Building_Data <- df[idx, ]
Implementation_Data <- df[-idx, ]

# Save
save(Model_Building_Data, file = "Model_Building_Data.rda")
save(Implementation_Data, file = "Implementation_Data.rda")
```

Place the generated `.rda` files in the same folder as `churn_analysis.Rmd` 
before knitting.

## Variables

| Column | Type | Description |
|--------|------|-------------|
| CustomerID | Character | Unique customer identifier |
| Gender | Factor | Male / Female |
| SeniorCitizen | Factor | 0 = No, 1 = Yes |
| Partner | Factor | Has a partner |
| Dependents | Factor | Has dependents |
| Tenure | Numeric | Months with the company |
| PhoneService | Factor | Has phone service |
| MultipleLines | Factor | Multiple phone lines |
| InternetService | Factor | DSL / Fiber optic / No |
| OnlineSecurity | Factor | Online security add-on |
| DeviceProtection | Factor | Device protection add-on |
| StreamingTV | Factor | Streaming TV add-on |
| StreamingMovies | Factor | Streaming movies add-on |
| Contract | Factor | Month-to-month / One year / Two year |
| PaperlessBilling | Factor | Paperless billing enabled |
| PaymentMethod | Factor | Electronic check / Mailed check / Bank transfer / Credit card |
| MonthlyCharges | Numeric | Current monthly charge ($) |
| TotalCharges | Numeric | Total charges to date ($) |
| Status | Factor | **Target variable** — Current / Left |

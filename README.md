# 📊 Customer Retention Cohort Analysis (Power BI)

https://app.powerbi.com/links/M4MEpd408D?ctid=463183f4-15eb-4683-b80c-f0738cdb2f8e&pbi_source=linkShare

This **Power BI project** performs **Customer Retention Cohort Analysis** using **DAX measures** and **Power Query transformations**. The report contains **two matrix visuals**:
1. **Customer Retention Volume** – Displays the absolute number of retained customers.
2. **Customer Retention Percentage** – Shows the percentage of customers retained per cohort.

---

## 📂 Data Sources & Preprocessing

### **Dataset:**
- **Subscription Cohort Analysis Data (CSV)**
  - Subscription Start (`created_date`), Cancellation (`canceled_date`)
  - Subscription Cost (`subscription_cost`)
  - Paid Status (`was_subscription_paid`)
  - Unique Customer Identifier (`customer_id`)

### **Power Query Transformations:**
- **Data Cleaning:** Removed unpaid subscriptions (`was_subscription_paid = "Yes"`)
- **Date Standardization:** Converted `created_date`, `canceled_date`, replaced nulls with `#date(2023, 9, 30)`
- **Feature Engineering for Cohorts:**
  - **Created Date (SOM):** Standardized cohort month (`Date.StartOfMonth([created_date])`)
  - **Month Span:** Number of months a customer was active
  - **Month List:** Generated a sequence of months since signup for retention tracking
  - **Expanded Month List:** Transformed each customer’s active duration into individual rows

---

## 📌 Data Model

- **Single Table Model:** All **subscription data** is stored in a single table (`Subscription Cohort Analysis`).
- **Measure Table:** Separate table used for calculated DAX measures.
- **No Relationships Required:** The dataset is structured to work without joins.

---

## 📌 DAX Measures & Explanation

### **1️⃣ Customer Retention Volume**
This measure calculates the **absolute number of retained customers** in each cohort over time.

```DAX
Customer Retention Volume = 
VAR CurrentMonth = SELECTEDVALUE('Subscription Cohort Analysis Data'[Month List])
VAR FirstJoinedMonth = SELECTEDVALUE('Subscription Cohort Analysis Data'[Created Date (SOM)])

VAR CustomerRetentionVolume = 
CALCULATE(
    DISTINCTCOUNT(
        'Subscription Cohort Analysis Data'[customer_id]
    ),
    FILTER(
        'Subscription Cohort Analysis Data',
        EOMONTH(
            'Subscription Cohort Analysis Data'[Created Date (SOM)], 0
        ) - 
        EOMONTH(FirstJoinedMonth, CurrentMonth
        )
    )
)

RETURN
CustomerRetentionVolume
```

### **2️⃣ Customer Retention Volume**
```
Customer Retention % = 
 VAR JoinedMonthVolume =
 CALCULATE(
    DISTINCTCOUNT(
        'Subscription Cohort Analysis Data'[customer_id]
    ),
    ALLEXCEPT(
        'Subscription Cohort Analysis Data',
        'Subscription Cohort Analysis Data'[Created Date (SOM)]
    )
 )

 VAR Ratio =
 DIVIDE(
    [Customer Retention Volume],
    JoinedMonthVolume
 )

 RETURN 
 Ratio
```

### 📊 Power BI Report Features

### 📈 Cohort Analysis Visuals

#### 1️⃣ Customer Retention Volume Matrix
- **Shows absolute number of retained customers per month.**
- **Identifies long-term subscribers vs. early churners.**

#### 2️⃣ Customer Retention Percentage Matrix
- **Displays retention rates over time.**
- **Highlights retention drop-offs across signup months.**

---

#### Retracing File Paths
- Modify path after downloading raw file and PBIX file to get the same analysis in your PBI Desktop which I did in **Power Query (Advanced Editor)**.
- Refresh dataset after updating paths.

---

### ⚙️ Tech Stack
- **Power BI** (Data Modeling, Visualizations)
- **Power Query (ETL, Data Transformation)**
- **DAX (Retention %, Customer Aggregations)**

---

### 📬 Contact

💼 **LinkedIn:** [https://www.linkedin.com/in/aneeque-data/)]  
📧 **Email:** [aaneequemushtaque@gmail.com]



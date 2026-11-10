# Data Dictionary

## Original columns (BankChurners.csv)

| Column | Meaning | Type |
|---|---|---|
| CLIENTNUM | Unique customer ID | ID |
| Attrition_Flag | Whether the customer left: *Existing Customer* or *Attrited Customer* | Category (target) |
| Customer_Age | Age in years | Number |
| Gender | M or F | Category |
| Dependent_count | Number of dependents | Number |
| Education_Level | Highest education (includes "Unknown") | Category |
| Marital_Status | Married, Single, Divorced or Unknown | Category |
| Income_Category | Annual income band (includes "Unknown") | Category |
| Card_Category | Blue, Silver, Gold or Platinum | Category |
| Months_on_book | Months the customer has been with the bank | Number |
| Total_Relationship_Count | Number of bank products held | Number |
| Months_Inactive_12_mon | Months inactive in the last 12 months | Number |
| Contacts_Count_12_mon | Number of contacts with the bank in the last 12 months | Number |
| Credit_Limit | Credit card limit | Number |
| Total_Revolving_Bal | Balance carried over on the card | Number |
| Avg_Open_To_Buy | Average unused credit over the last 12 months | Number |
| Total_Amt_Chng_Q4_Q1 | Change in transaction amount, Q4 compared with Q1 | Ratio |
| Total_Trans_Amt | Total transaction amount over the last 12 months | Number |
| Total_Trans_Ct | Total number of transactions over the last 12 months | Number |
| Total_Ct_Chng_Q4_Q1 | Change in transaction count, Q4 compared with Q1 | Ratio |
| Avg_Utilization_Ratio | Share of the credit limit in use (0 to 1) | Ratio |

## Renamed columns (used in the notebook)

| Original | New name |
|---|---|
| CLIENTNUM | customer_id |
| Attrition_Flag | customer_status |
| Months_Inactive_12_mon | months_inactive |
| Contacts_Count_12_mon | contacts_count |
| Total_Relationship_Count | products_held |
| Months_on_book | tenure_months |
| Avg_Open_To_Buy | available_credit |
| Total_Revolving_Bal | revolving_balance |
| Total_Trans_Amt | trans_amount |
| Total_Trans_Ct | trans_count |
| Total_Amt_Chng_Q4_Q1 | amt_change_q4_q1 |
| Total_Ct_Chng_Q4_Q1 | ct_change_q4_q1 |
| Avg_Utilization_Ratio | utilization_ratio |

All other columns were only changed to lowercase. The `$` sign was also removed from the income labels (for example "60K - 80K") so chart labels display correctly.

## Columns created in this project

| Column | How it is made | Use |
|---|---|---|
| churn_flag | 1 if the customer left, 0 if still a customer | Churn rate = average of this column |
| age_group | Under 36, 36-45, 46-55, 56+ | Compare age groups |
| tenure_group | Up to 2 yrs, 2-3 yrs, 3-4 yrs, 4+ yrs | Compare new and long-standing customers |
| avg_trans_value | trans_amount divided by trans_count | Average spend per purchase |
| inactive_flag | 1 if inactive for 3+ months | Warning sign |
| low_trans_flag | 1 if 60 or fewer transactions | Warning sign |
| high_contact_flag | 1 if 3+ contacts with the bank | Warning sign |
| few_products_flag | 1 if 1 or 2 products held | Warning sign |
| risk_score | Sum of the four warning signs (0 to 4) | Risk ranking |
| risk_level | Low (0-1), Medium (2), High (3-4) | Customer segment |
| risk_order | Low = 1, Medium = 2, High = 3 | Correct sort order in Power BI |

The notebook also creates `trans_group`, `limit_group` and `spend_group` for charts. They are not in the exported file.

# Methodology

## 1. Data cleaning

| Check | Result | Decision |
|---|---|---|
| Duplicate rows and customer IDs | 0 | No action needed |
| Missing values (blank) | 0 | No action needed |
| Hidden missing values ("Unknown") | Education 1,519 (15.0%), marital status 749 (7.4%), income 1,112 (11.0%) | Kept as a separate category. Deleting would remove up to 1 in 6 customers and could bias results. |
| Outliers in credit limit and transaction amount | Present | Kept. They are genuine high-value customers, the group a bank most wants to retain. |
| Customer age | 26 to 73 | No errors found |
| Column names | Mixed case, long names | Lowercased and shortened |
| Income labels | Contain `$` signs | Removed so chart labels display correctly |

## 2. Why churn *rate*, not counts

Only about 16% of customers churned (1,627 of 10,127). Large groups always contain more churned customers, so raw counts would mislead. Every comparison uses the **churn rate inside each group**: the average of `churn_flag`.

## 3. Feature engineering

Groups (age, tenure) turn many separate values into a few meaningful bands, so results are easy to read. `avg_trans_value` separates customers who make many small purchases from those who make a few large ones. The inactivity cut-off of 3 months was chosen as a simple, explainable definition of "drifting away" and was supported by the data (22.0% churn vs 11.2%).

## 4. Risk segmentation (rule-based)

Each customer scores **1 point per warning sign**:

| Warning sign | Rule | Evidence |
|---|---|---|
| Low card use | 60 or fewer transactions | Churn 31.5% to 37.0%, vs 5.8% or less above 60 |
| Inactive | 3 or more months inactive | 22.0% vs 11.2% |
| Many bank contacts | 3 or more contacts | 20.1% to 33.5% (and 100% at 6 contacts, only 54 customers), vs 1.8% with none |
| Few products | 1 or 2 products | 25.6% to 27.8%, vs 10.5% to 17.4% with 3 or more |

Scores are grouped into **Low (0-1), Medium (2), High (3-4)**.

### Results

| Risk score | Customers | Churn rate |
|---|---|---|
| 0 | 1,332 | 0.9% |
| 1 | 3,664 | 5.1% |
| 2 | 3,483 | 16.8% |
| 3 | 1,437 | 44.5% |
| 4 | 211 | 95.7% |

| Risk level | Customers | Churn rate |
|---|---|---|
| Low | 4,996 | 4.0% |
| Medium | 3,483 | 16.8% |
| High | 1,648 | 51.1% |

The High group holds 842 of the 1,627 churned customers (51.8%).

Churn rises steadily with each extra warning sign, which supports the rules. A rule-based score was chosen over machine learning because it is transparent: a retention manager can see exactly why a customer is flagged.

## 5. Limitations

- **Snapshot data without dates.** No trends over time, and no proof of cause. Low usage may be a symptom of a customer already leaving.
- **No hold-out test.** The rules were designed and checked on the same data, so performance on new customers is likely lower. Treat the score as a screening tool.
- **Chosen cut-offs.** Selected from the charts; other reasonable cut-offs may give slightly different numbers.
- **Small groups.** 20 Platinum cardholders, 29 customers with 0 inactive months, 54 customers with 6 contacts. Treat their rates as indicative only.
- **Contacts are ambiguous.** More contacts may mean complaints, but the reason for contact is not in the data.
- **The "what if" example** (10% of High risk churners kept, about 84 customers) is an assumption for illustration, not a result.

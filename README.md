# 🔍 Operation Clearwater — Transaction Fraud Detection & Risk Intelligence

> A simulated bank fraud investigation built end-to-end in Python across 195,276 transactions from a fictional mid-sized commercial bank operating across 12 countries.

---

## 📌 Project Overview
NorthAxis Bank's compliance hotline recorded a 340% spike in fraud-related complaints in Q3 2024. Internal audit flagged suspicious outflows concentrated in a 6-week window. This project simulates the role of a Junior Data Analyst on the Risk Intelligence team tasked with investigating transaction patterns, surfacing anomalies, profiling high-risk accounts, and delivering a board-ready risk report.

- **Tool used**: Python (pandas, numpy, matplotlib, seaborn)
- **Data period**: January – September 2024
- **Dataset size**: 195,276 transactions | 8,000 customers | 6 relational tables

---

## 🗂️ Dataset Structure
| Tables | Rows | Description
|------|----|----------|
|`fact_transaction`| 195,276 | Core transaction log amounts, channels, flags, timestamps |
|`dim_customer`| 8000 | Customer demographics, KYC status, fraud target flag |
|`dim_account`| 11,312 | Account types, balances, credit limits |
|`dim_merchant`| 45 | Merchant names, categories, shell merchant flag, risk rating |
|`dim_location`| 64 | Countries, cities, high-risk country flag |
|`dim_date`| 274 | Calendar table — weekends, quarters, month-end flags |

---

## 🧱 Project Structure (6 Analytical Tasks)

### Task 1 — Transaction Overview & Baseline KPIs
Established what normal looks like before hunting anomalies.

- **195,276** total transactions | **$355,057,585.98** total value
- Date range: Jan 1 – Sep 30, 2024
- **19,741** pre-flagged transactions in the raw data
- Mobile Banking was the highest volume channel (77,973 transactions)
- Wire Transfers and Transfers accounted for **100% of all flagged transaction types**
- Fraud activity visibly escalated from May 2024 onward — flagged transactions jumped from ~400/month (Jan–Apr) to 4,733 in July alone
---
 
### Task 2 — Anomaly Detection & Velocity Checks
Flagged what breaks the pattern across 4 fraud signals:
 
| Signal | Finding |
|---|---|
| Off-hours activity (1AM–4AM) | 16,230 transactions | $79.5M | 10,062 flagged |
| Amount outliers (>3× avg of $1,818) | 5,353 transactions above $19,437 threshold | $177M total |
| Velocity anomalies (<10 mins apart) | 4,617 rapid transactions across 283 customers |
| Geographic mismatch | Explored but noted as weak standalone signal — useful only when combined with velocity (impossible travel) |
 
---
 
### Task 3 — Customer Risk Profiling
Built individual behavioural baselines per customer and flagged deviations from their own history.
 
- **7,216** customers with amount spikes (max transaction > 3× their own average)
- **247** customers with repeat fraud flags (>3 flagged transactions)
- **229** customers with persistent off-hours activity (>5 times)
- **596** customers with impossible travel events (different location within 30 minutes)
- **5,295** impossible travel events detected total
---
 
### Task 4 — Merchant & Channel Risk Scoring
Ranked merchants and channels by fraud concentration.
 
**Shell Merchants (100% fraud rate):**
GlobalTrade Ltd, FastBridge Finance, NovaPay Solutions, PrimeFin Corp, SwiftFunds Inc, TrustEx Global, Apex Transfers, ClearPath Remit were all flagged at 100% fraud rate with High risk rating.
 
**Channel fraud rates (overall):**
 
| Channel | Fraud Rate |
|---|---|
| Mobile Banking | 14.36% |
| Web Banking | 11.62% |
| Branch | 5.45% |
| POS | 3.29% |
| ATM | 2.51% |
 
**Merchant category:** Shell Merchant category had a 100% fraud rate vs ~3.2–3.6% across all legitimate categories.
 
---
 
### Task 5 — Fraud Risk Scoring Model
Built a weighted composite risk score per customer using 5 flags:
 
| Flag | Weight |
|---|---|
| Repeat fraud transactions | 40 |
| Impossible travel | 30 |
| Amount spike | 20 |
| Risky merchant transactions | 15 |
| Off-hours activity | 10 |
 
**Risk tier distribution (8,000 customers):**
 
| Tier | Count |
|---|---|
| 🔴 Critical | 231 |
| 🟠 High | 418 |
| 🟡 Medium | 6,784 |
| 🟢 Low | 514 |
 
---
 
### Task 6 — Executive Risk Report & Recommendations
 
**Exposure by tier:**
 
| Tier | Customers | Flagged Transactions | Amount at Risk |
|---|---|---|---|
| Critical | 231 | 19,211 | $258,297,600 |
| High | 418 | 530 | $14,044,610 |
 
**Channel fraud rates among flagged accounts:**
 
| Channel | Fraud Rate |
|---|---|
| Mobile Banking | 72.78% |
| Web Banking | 69.28% |
| Branch | 50.93% |
| POS | 37.76% |
| ATM | 31.56% |
 
**Recommendations:**
1. **FREEZE** — Immediately suspend 231 Critical accounts
2. **RESTRICT** — Limit digital channel transactions above $500 pending review
3. **REVIEW** — KYC re-verification for all High & Critical tier customers
4. **MONITOR** — Real-time velocity alerts for accounts scoring above 20
5. **ESCALATE** — Refer Critical accounts to regulatory compliance team
---
 
## 🛠️ How to Run
 
```bash
# Install dependencies
pip install pandas numpy matplotlib seaborn
 
# Launch notebook
jupyter notebook Operation_Clearwater.ipynb
```
 
Place all 6 CSV files in the same directory as the notebook and update file paths in Cell 2.
 
---
 
## 📁 Output Files
 
| File | Description |
|---|---|
| `fraud_watchlist.csv` | All High + Critical customers with scores |
| `freeze_list.csv` | Top 20 Critical accounts sorted by risk score + flagged transactions |
| `channel_restriction.csv` | Channel fraud rates for flagged customer subset |
| `exposure_details.csv` | Exposure summary by risk tier |
| `executive_risk_report.txt` | Board-ready text summary |
 
---
 
## 🧠 Key Learnings
 
- Baseline first — you can't detect anomalies without knowing what normal looks like
- Geography alone is a weak fraud signal; it only becomes powerful combined with velocity (impossible travel)
- Proportion matters more than volume — a 100% fraud rate on 1,698 transactions tells a clearer story than 19,741 flags spread across 195K
- Risk scoring is a business decision, not just a math exercise — thresholds should reflect the cost of missing real fraud vs. the cost of false positives
---
 
## 📎 Original Brief
 
Project scenario sourced from a public challenge shared on Twitter/X. Solved independently using Python instead of the suggested SQL Server.







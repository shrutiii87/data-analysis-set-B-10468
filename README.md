# 🎧 Customer Support Quality Analysis

A customer support organisation wants to understand where and why tickets are taking too long to resolve, so it can decide which department, team, and contact channel needs operational attention. This project analyzes 12 support tickets across 4 teams and 3 channels (Jan–Mar) using SQL, Python, Excel, and Power BI, and answers two business questions.

---

**Student Name:** Shruti Bhawsar
**GR No:** 10468
**Assigned Set:** Set B
**Repository:** `data-analysis-set-B_10468`

---

🔨 Tools used :-

<div>

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white"/>
<img src="https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
<img src="https://img.shields.io/badge/SQL-00758F?style=for-the-badge&logo=postgresql&logoColor=white"/>
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/>
<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
<img src="https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=plotly&logoColor=white"/>
<img src="https://img.shields.io/badge/INDEX--MATCH-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white"/>
<img src="https://img.shields.io/badge/COUNTIFS-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white"/>
<img src="https://img.shields.io/badge/AVERAGEIFS-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white"/>

</div>

---

## 📌 Overview

Customer Support Quality Analysis to identify which department has the slowest resolution times and which contact channel produces the most SLA breaches. Done across four modules — Excel, Power BI, SQL, and Python — using `tickets` and `teams` datasets linked by `team_id`. Includes duplicate removal, `breach_flag` derivation, and analysis by department, team, channel, and month.

---

## 🎯 Business Objective

The business objective of this project is to analyze support-ticket resolution patterns to answer two key questions — which department (Service vs Technical) is slowest to resolve tickets and breaches the 24-hour SLA most often, and which channel (Chat, Phone, Email) generates the most breaches. The analysis supports operational decisions by identifying the teams that average more than 24 hours, tracking resolution time over the three months, and checking how SLA breaches relate to customer satisfaction.

---

## 🎬 Project Demo

📹 Add a link to your project walkthrough video here.

---

## 📁 Dataset Filenames

| File | Description |
|---|---|
| `setup.sql` | Creates `teams` and `tickets` tables, inserts raw data (4 + 12 rows) |
| `queries.sql` | 4 SQL queries (S2a, S2b, S2c, S3 diagnostic) |
| `tickets.csv` | Raw ticket records (13 rows incl. 1 duplicate) |
| `teams.csv` | Raw team lookup table |
| `clean_data.csv` | Cleaned, merged Python output with `breach_flag` |
| `python_summary.csv` | Python department-level SLA breach summary |
| `Python_analysis.ipynb` | Python notebook |
| `Python_chart.png` | Monthly average resolution hours chart |
| `S2a_avg_resolution_by_department.csv` | SQL — average resolution hours by department |
| `S2b_teams_breaching_sla.csv` | SQL — teams averaging > 24 hours |
| `S2c_top_two_channels_by_breach.csv` | SQL — top 2 channels by breach count |
| `S3_team_ticket_diagnostic.csv` | SQL — unmatched team / orphan check |
| `excel_analysis.xlsx` | Excel: `Raw`, `Lookup`, `Clean`, `Summary` sheets |
| `Screenshot_2026-10-03_113715.png` | Power BI dashboard screenshot |

---

### 📖 Data Dictionary

**`teams` (4 rows)**

| Column | Type | Meaning |
|---|---|---|
| `team_id` | Text (PK) | `T1`–`T4` |
| `team` | Text | Team name (AccountCare, BillingHelp, AppSupport, DeviceHelp) |
| `department` | Text | `Service` or `Technical` |

**`tickets` (13 raw rows → 12 after cleaning)**

| Column | Type | Meaning |
|---|---|---|
| `ticket_id` | Integer (PK) | Unique ticket ID |
| `month` | Text | `Jan` / `Feb` / `Mar` |
| `team_id` | Text (FK) | → `teams.team_id` |
| `channel` | Text | `Email` / `Chat` / `Phone` |
| `resolution_hours` | Numeric | Hours taken to resolve the ticket |
| `satisfaction` | Numeric | Customer satisfaction score (2–5) |

**Derived columns**

| Column | Type | Meaning |
|---|---|---|
| `breach_flag` | Integer (0/1) | `1` if `resolution_hours > 24`, else `0` |

**Full cleaned dataset (12 rows):**

| ticket_id | month | team_id | channel | resolution_hours | satisfaction | team | department | breach_flag |
|---|---|---|---|---|---|---|---|---|
| 1 | Jan | T1 | Email | 12 | 4 | AccountCare | Service | 0 |
| 2 | Jan | T2 | Chat | 28 | 3 | BillingHelp | Service | 1 |
| 3 | Jan | T3 | Phone | 36 | 2 | AppSupport | Technical | 1 |
| 4 | Jan | T4 | Email | 20 | 4 | DeviceHelp | Technical | 0 |
| 5 | Feb | T1 | Chat | 8 | 5 | AccountCare | Service | 0 |
| 6 | Feb | T2 | Phone | 30 | 3 | BillingHelp | Service | 1 |
| 7 | Feb | T3 | Email | 18 | 4 | AppSupport | Technical | 0 |
| 8 | Feb | T4 | Chat | 40 | 2 | DeviceHelp | Technical | 1 |
| 9 | Mar | T1 | Phone | 16 | 4 | AccountCare | Service | 0 |
| 10 | Mar | T2 | Email | 22 | 4 | BillingHelp | Service | 0 |
| 11 | Mar | T3 | Chat | 32 | 3 | AppSupport | Technical | 1 |
| 12 | Mar | T4 | Phone | 24 | 5 | DeviceHelp | Technical | 0 |

---

## 🧹 Cleaning Steps Taken

- Loaded `tickets.csv` + `teams.csv`; checked shape, dtypes, nulls (none found).
- Found and removed 1 exact duplicate row (`ticket_id = 12`): **13 → 12 rows**.
- Left-merged `tickets` with `teams` on `team_id`.
- Validated merge: 12 rows, zero missing `department` (confirmed via SQL diagnostic S3 → every team has 3 matched tickets, `unmatched_team_flag = 0`).
- Derived `breach_flag`.
- Exported `clean_data.csv` and `python_summary.csv`.

### 📐 Metric Definitions

```
breach_flag = 1 if resolution_hours > 24 else 0

sla_breach_rate =
    COUNT(resolution_hours > 24) / COUNT(total tickets) * 100
```

Example (Technical, 6 tickets): 3 breached → `3/6*100 = 50.00%`; Service: `2/6*100 = 33.33%`

---

## 🛠️ Tools & Versions Used

| Tool | Version |
|---|---|
| SQL Engine | PostgreSQL 16.x |
| Python | 3.x |
| pandas | 2.x |
| matplotlib | 3.x |
| Jupyter Notebook | 7.x |
| Microsoft Excel | Microsoft 365 (desktop) |
| Power BI | Power BI Desktop |

> Run `pip freeze > requirements.txt` and paste exact pinned versions here.

---

## 📊 TASK :- 1 Excel Sheet Guide

| Sheet | Purpose |
|---|---|
| `Raw` | Original, unedited `tickets` data (13 rows, incl. duplicate `ticket_id 12`) |
| `Lookup` | Team reference table (`team_id`, `team`, `department`) used for lookups |
| `Clean` | Deduplicated data (12 rows) with `department` pulled via `INDEX/MATCH` and `breach_flag` via `IF(E>24,1,0)`; includes a Row Count Check panel (Before cleaning 13 → After cleaning 12 → Duplicates removed 1) |
| `Summary` | Formula-based summary — breached tickets by channel (`COUNTIFS`) and average resolution hours by department × month (`AVERAGEIFS`) |

**Excel results**

| Channel | Breached Tickets |
|---|---|
| Email | 0 |
| Chat | 3 |
| Phone | 2 |

| Department | Jan | Feb | Mar |
|---|---|---|---|
| Service | 20.0 | 19.0 | 19.0 |
| Technical | 28.0 | 29.0 | 28.0 |

---

## 🧮 TASK :- 2 SQL Setup & Query Execution Steps

Run `setup.sql` first, then `queries.sql` (PostgreSQL 16):

```bash
psql -U <user> -d <database> -f setup.sql
psql -U <user> -d <database> -f queries.sql
```

**`setup.sql`** — creates tables + loads data (key part):

```sql
CREATE TABLE tickets (
    ticket_id INTEGER PRIMARY KEY,
    month VARCHAR(10) NOT NULL,
    team_id VARCHAR(10) NOT NULL,
    channel VARCHAR(20) NOT NULL,
    resolution_hours NUMERIC(10,2) NOT NULL,
    satisfaction NUMERIC(3,1),
    CONSTRAINT fk_tickets_team
        FOREIGN KEY (team_id) REFERENCES teams(team_id)
);
```

**`queries.sql`** — the core resolution-time query (repeated per grouping):

```sql
SELECT
    tm.department,
    ROUND(AVG(t.resolution_hours), 2) AS avg_resolution_hours
FROM tickets AS t
JOIN teams AS tm ON t.team_id = tm.team_id
GROUP BY tm.department
ORDER BY avg_resolution_hours DESC;
```

Same pattern is reused for the team (`HAVING AVG(...) > 24`), channel (`WHERE resolution_hours > 24 ... LIMIT 2`, alphabetical tie-break), and unmatched-team (`LEFT JOIN`) queries — see `queries.sql` for the full set.

**SQL results**

| Query | Result |
|---|---|
| S2a — Avg resolution by department | Technical **28.33** h · Service **19.33** h |
| S2b — Teams with avg > 24 h | AppSupport **28.67** · DeviceHelp **28.00** · BillingHelp **26.67** |
| S2c — Top 2 channels by breach count | Chat **3** · Phone **2** |
| S3 — Diagnostic | All 4 teams matched 3 tickets each; `unmatched_team_flag = 0` |

---

## 🐍 TASK :- 3 Python Environment Setup & Run Instructions

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install pandas matplotlib jupyter
jupyter notebook Python_analysis.ipynb
```

**Core cleaning + metric logic** (from the notebook):

```python
tickets = tickets.drop_duplicates()
merged = tickets.merge(teams, on="team_id", how="left")

assert len(merged) == 12
assert merged["department"].isna().sum() == 0

merged["breach_flag"] = (merged["resolution_hours"] > 24).astype(int)

merged.to_csv("clean_data.csv", index=False)
```

Notebook stages: `P1` load/clean/merge → `P2` derive `breach_flag` + department & team summaries → `P3` monthly average resolution chart → export CSVs.

**Python results — department summary (`python_summary.csv`)**

| department | total_tickets | breached_count | sla_breach_rate_percent |
|---|---|---|---|
| Service | 6 | 2 | 33.33 |
| Technical | 6 | 3 | 50.00 |

**Python results — team summary**

| team | total_tickets | breached_count | breach_rate_percent |
|---|---|---|---|
| AccountCare | 3 | 0 | 0.00 |
| BillingHelp | 3 | 2 | 66.67 |
| AppSupport | 3 | 2 | 66.67 |
| DeviceHelp | 3 | 1 | 33.33 |

**Monthly average resolution hours:** Jan **24.0** → Feb **24.0** → Mar **23.5**

![Monthly Average Resolution Hours](Python_chart.png)

---

## 📊 TASK :- 4 Power BI Data-Source Refresh Instructions

![Power BI dashboard](Screenshot_2026-10-03_113715.png)

**Dashboard contents:** KPI cards (Ticket Count **12**, SLA Breach Rate **41.67%**, Avg Satisfaction **3.58**), a channel slicer (Chat / Phone / Email), Avg Satisfaction by department bar chart, and Average Resolution Hours by month area chart.

1. Open the `.pbix` in Power BI Desktop.
2. **Home → Transform Data → Data Source Settings**.
3. Select the `clean_data.csv` source → **Change Source…**.
4. Point to the cloned repo path:
   ```
   .../data-analysis-set-B_10468/clean_data.csv
   ```
5. **Refresh** on the Home ribbon.
6. If refresh errors, check that column headers still match (`month`, `channel`, `department`, `resolution_hours`, `satisfaction`, `breach_flag`).

---

## 📈 Numeric Findings & Recommendation

- **Finding 1:** **Technical** is the slowest department — average resolution **28.33 h vs 19.33 h** for Service (a 9-hour gap). Its SLA breach rate is **50.00% (3 of 6)** vs **33.33% (2 of 6)** for Service.
- **Finding 2:** **Chat** causes the most SLA breaches — **3 of the 5 total breaches**, followed by Phone with **2**; Email has **0**. Chat also has the highest average resolution time (27.0 h), ahead of Phone (26.5 h) and Email (18.0 h).
- **Team view:** Three of four teams average above 24 hours — AppSupport (28.67 h), DeviceHelp (28.00 h), BillingHelp (26.67 h). Only AccountCare (12.0 h, 0 breaches) stays within SLA.
- **Satisfaction link:** Breached tickets average **2.60** satisfaction vs **4.29** for tickets resolved within SLA. Department satisfaction follows the same pattern: Service **3.83** vs Technical **3.33**.
- **Trend:** Average resolution hours by month: **Jan 24.0 → Feb 24.0 → Mar 23.5** — essentially flat. Breached tickets by month: **Jan 2, Feb 2, Mar 1**.

**Recommendation:** Prioritize the Technical department (AppSupport and DeviceHelp) and the Chat channel — they drive the longest resolution times and the most breaches, and breached tickets score far lower on satisfaction. The monthly trend is flat, so the problem is persistent rather than improving on its own.

---

## 🔗 Cross-Tool Reconciliation

**Metric:** Total SLA-breached tickets (`resolution_hours > 24`)

| Tool | Value |
|---|---|
| Python | 5 |
| SQL | 5 (Chat 3 + Phone 2) |
| Excel | 5 (Email 0 + Chat 3 + Phone 2) |
| Power BI | 5 (41.67% × 12 tickets) |

**Metric:** Overall SLA breach rate — all 12 tickets

| Tool | Value |
|---|---|
| Python | 41.666...% (rounds to 41.67%) |
| Excel | 5 ÷ 12 = 41.67% |
| Power BI | 41.67% |

✅ All tools match once rounded to 2 decimals. Power BI shows a Ticket Count of 12, confirming the duplicate `ticket_id 12` was removed there too. (For reference, leaving the duplicate in would have given 6 ÷ 13 = 46.15%.)

---

## ⚠️ Assumptions & Limitations

- Small sample (12 rows) — findings are directional, not statistically robust.
- A breach is defined strictly as `resolution_hours > 24`; a ticket resolved in exactly 24 hours (ticket 12) is not a breach.
- BillingHelp and AppSupport tie for the highest team breach rate (66.67%); the notebook's `idxmax` returns the first of the tied teams (BillingHelp).
- Only 1 duplicate found; no nulls or unmatched teams after cleaning.

---

## 🙏 Thank You

Thank you for taking the time to explore this project!
Your feedback, suggestions, and contributions are always welcome.

⭐ If you found this project helpful, don't forget to **star the repository** and share it with others.

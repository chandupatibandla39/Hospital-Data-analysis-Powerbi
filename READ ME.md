# 🏥 Hospital Data Analysis – Power BI Dashboard

An end-to-end Power BI project analysing **3,000 patient records across 10 hospital departments** (Jan 2025 – May 2026). The report has 9 pages covering readmissions, mortality, ER delays, revenue, surgery cancellations, patient satisfaction, blood bank usage, diagnostic delay and follow-up compliance.

> **Tools:** Power BI Desktop · DAX · Data Modelling · Data Visualization
> **Dataset:** `Healthcare_Master` – 3,000 rows × 34 columns *(add source here, e.g. simulated / practice dataset)*

---

## 🎯 Business Questions

1. Which departments and patients have the highest readmission rates, and does follow-up compliance matter?
2. How does mortality vary by department and severity, and is diagnosis delay related to it?
3. How long do patients wait in the emergency room, and what drives it?
4. How does treatment cost vary by department and insurance type?
5. Why are surgeries cancelled, and which departments are most affected?
6. What drives patient satisfaction?
7. How efficiently is blood inventory used, and how much expires?
8. Do doctor experience and number of tests affect diagnosis delay?
9. Who completes follow-up visits, and who doesn't?

---

## 📊 Dashboard Pages & Key Findings

| # | Page | Key Finding |
|---|------|-------------|
| 1 | **Readmission Analysis** | Overall readmission is **29.8%**. ICU (34.6%), Cardiology (34.3%) and Emergency (33.9%) are highest. Critical patients who missed follow-up were readmitted at **46.3% vs 34.2%** when follow-up was completed (~12 points higher). |
| 2 | **Mortality** | Overall mortality is **13.6%**. Endocrinology (17.8%) and Emergency (15.7%) are highest; Pulmonology (10.7%) is lowest. |
| 3 | **Emergency Delay** | Average wait is **75.7 minutes**, and **63.5%** of patients wait more than 55 minutes. Nephrology has the longest average wait (80.6 min). |
| 4 | **Revenue Analysis** | Monthly revenue trend, average cost per patient by department and insurance type, and cost of surgery cases. |
| 5 | **Surgery Cancellation** | **106 of 1,077** required surgeries were cancelled (**9.8%**). Top reasons: Equipment Shortage and Patient Unfit (26 each), then Doctor Unavailable (20). |
| 6 | **Patient Satisfaction** | Average satisfaction is **2.23 / 5**. Longer waits go with lower satisfaction (**correlation ≈ -0.56**). Patients with a cancelled surgery score lower (1.97 vs 2.23). |
| 7 | **Blood Bank** | **175,375** units available, **133,478** used (**76.1%**) and **20,721** expired (**11.8%**). |
| 8 | **Diagnostic Error** | Patients who died had a longer average diagnosis delay (**39.2 h vs 34.8 h**). Doctor experience shows essentially no relationship with delay (r ≈ 0.00). |
| 9 | **Follow-up Compliance** | **70%** of patients completed follow-up; breakdown by age group, city, insurance type and disease. |

---

## 🧮 DAX Measures

```DAX
Total Patients        = COUNTROWS(Healthcare_Master)
Total Readmitted      = CALCULATE(COUNTROWS(Healthcare_Master), Healthcare_Master[Readmitted] = "Yes")
Readmission Rate %    = DIVIDE([Total Readmitted], [Total Patients], 0)

Total Mortality       = CALCULATE(COUNTROWS(Healthcare_Master), Healthcare_Master[Mortality] = "Yes")
Mortality Rate %      = DIVIDE([Total Mortality], [Total Patients], 0)

Average Wait Time     = AVERAGE(Healthcare_Master[Waiting_Time_Minutes])
Long Wait Patients    = CALCULATE(COUNTROWS(Healthcare_Master), Healthcare_Master[Waiting_Time_Minutes] > 55)
Long Wait Rate %      = DIVIDE([Long Wait Patients], [Total Patients], 0)

Total Revenue         = SUM(Healthcare_Master[Treatment_Cost])
Average Revenue per Patient = AVERAGE(Healthcare_Master[Treatment_Cost])

Surgeries Required    = CALCULATE(COUNTROWS(Healthcare_Master), Healthcare_Master[Surgery_Required] = "Yes")
Cancelled Surgeries   = CALCULATE(COUNTROWS(Healthcare_Master), Healthcare_Master[Surgery_Cancelled] = "Yes")
Cancellation Rate %   = DIVIDE([Cancelled Surgeries], [Surgeries Required], 0)

Average Satisfaction  = AVERAGE(Healthcare_Master[Satisfaction_Score])

Usage Rate %          = DIVIDE([Total Used], [Total Available], 0)
Expiry Rate %         = DIVIDE([Total Expired], [Total Available], 0)
Shortfall             = [Total Available] - [Total Used]

Average Diagnosis Delay = AVERAGE(Healthcare_Master[Diagnosis_Delay_Hours])
Avg Delay - Mortality Yes = CALCULATE([Average Diagnosis Delay], Healthcare_Master[Mortality] = "Yes")
Avg Delay - Mortality No  = CALCULATE([Average Diagnosis Delay], Healthcare_Master[Mortality] = "No")

Followup Completion Rate % = DIVIDE([Total Followup Completed], [Total Patients], 0)
```

*(Supporting sum measures for blood units and follow-up counts are included in the `.pbix`.)*

---

## 🗂️ Data Dictionary (key columns)

| Group | Columns |
|-------|---------|
| Patient | `Patient_ID`, `Age`, `Gender`, `City`, `Disease`, `Age Group` (derived) |
| Admission | `Department`, `Admission_Date`, `Discharge_Date`, `Stay_Days`, `Severity_Level` |
| Clinical | `Oxygen_Level`, `Blood_Pressure`, `Heart_Rate`, `Tests_Done`, `Diagnosis_Delay_Hours` |
| Doctor | `Doctor_ID`, `Doctor_Experience_Years` |
| Operations | `Waiting_Time_Minutes`, `Insurance_Type`, `Treatment_Cost`, `Satisfaction_Score` |
| Surgery | `Surgery_Required`, `Surgery_Cancelled`, `Cancellation_Reason` |
| Blood bank | `Blood_Group`, `Blood_Units_Available`, `Blood_Units_Used`, `Blood_Units_Expired`, `Emergency_Blood_Request` |
| Outcomes | `Followup_Completed`, `Previous_Revisit`, `Readmitted`, `Mortality` |

**Data quality notes:** `Doctor_ID` (45), `Diagnosis_Delay_Hours` (50) and `Treatment_Cost` (70) contain missing values. *(Add how you handled them.)*

---

## 💡 Recommendations

- **Enforce follow-up for high-severity discharges.** Missed follow-up is associated with about 10–12 points higher readmission across all severity levels.
- **Cut ER waiting time.** Waiting time has the strongest link to satisfaction in this data.
- **Fix equipment availability.** It is the joint top cause of surgery cancellations.
- **Reduce blood expiry.** About 1 in 9 units expires; rebalance stock by blood group.
- **Look at diagnosis delay in high-mortality departments.**

> Findings show association, not proven causation.

---

## 🚀 How to Use

1. Clone the repo: `git clone https://github.com/chandupatibandla39/hospital-data-analysis-powerbi.git`
2. Open `hospital_data_analysis.pbix` in **Power BI Desktop**.
3. Use the slicers (Department, Severity, Insurance Type, Blood Group, Age Group) to explore.

---

## 📁 Repository Structure

```
hospital-data-analysis-powerbi/
├── hospital_data_analysis.pbix
├── dashboard_preview.pdf
├── screenshots/
│   ├── 01_readmission.png
│   └── ...
└── README.md
```

---

## 👤 Author

**Purna Chandra Sekhar Patibandla**
[LinkedIn](https://www.linkedin.com/in/purna-chandra-sekhar-patibandla) · [GitHub](https://github.com/chandupatibandla39) · chandupatibandla39@gmail.com

# 📊 Power BI 360 Project — Atliq Hardware Ltd.

Welcome to my **Power BI 360-Degree Dashboard Project**, a business intelligence solution created for **Atliq Hardware Ltd.**, an electronic hardware company transitioning from Excel to modern data analytics.

---

## 🏢 About the Company

**Atliq Hardware Ltd.** is a manufacturer of electronic hardware components catering to B2B clients. Initially reliant on Excel-based reporting, the company found itself falling behind as competitors began leveraging data analytics. Determined to reclaim their edge, Atliq sought a **Data Analyst team** to build efficient, insightful dashboards that drive smarter decisions—**that’s where this project comes in**.

---

## 🚀 Project Objective

To empower Atliq Hardware with **data-driven insights** across five business domains:
**Live Dashboard** Link: https://app.powerbi.com/view?r=eyJrIjoiYWQxOTcyZmItN2VkNi00ZTQyLTllYzEtMWFhOGIxYmVlOTEwIiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9&pageName=ReportSection0e765c0061580b067c73

- 🧾 Finance  
- 💼 Sales  
- 📢 Marketing  
- 🧑‍💼 Executive/Leadership  
- 🔗 Supply Chain  

All dashboards are interconnected and designed for a **360-degree view of performance**, helping stakeholders identify gaps, benchmark progress, and forecast outcomes.

---

## 🛠️ Tools & Technologies

- **Power BI** – for dashboard creation & data modeling  
- **DAX** – for creating advanced calculated measures  
- **SQL** – for data preprocessing (files included)  
- **Excel** – as a source format used initially by Atliq  
- **GitHub** – to document and share the full analytics workflow  

---

## 📁 Project Structure

```
PowerBI-360-DAX/
│
├── 📁 Finance
├── 📁 Sales
├── 📁 Marketing
├── 📁 Executive
├── 📁 Supply Chain
├── 📁 Shared_UI
├── gdb041.sql.gz  ← Raw database schema 1
├── gdb056.sql.gz  ← Raw database schema 2
└── README.md  ← (You are here!)
```

Each folder contains **DAX formulas (.dax files)** that were used to build KPIs, trend visuals, and target/benchmark comparisons.

---

## 🔍 Highlights

✅ Clean **DAX Measures** categorized by dashboard  
✅ Clear **Titles and Dynamic Texts** for storytelling visuals  
✅ Smart use of **SWITCH, CALCULATE, SAMEPERIODLASTYEAR, VAR**  
✅ Includes performance indicators like **GM %, NP %, Forecast Accuracy %**  
✅ Ready to plug into your Power BI report  
✅ Includes **two SQL database schemas** used for data source modeling

---

## 📈 Sample KPIs Created

- **Net Sales**, **Gross Sales**, **Manufacturing Cost**  
- **Gross Margin %**, **Net Profit %**, **Operational Expense**  
- **Market Share %**, **Forecast Accuracy %**, **Risk Flagging (OOS/EI)**  
- **Top/Bottom Performers**, **Dynamic P&L View**

---

## 🧠 Business Impact

> By switching from Excel to a centralized Power BI solution,  
> **Atliq Hardware Ltd.** can now:  
> - Monitor key KPIs in real time  
> - Benchmark vs. previous year and targets  
> - Reduce manual reporting effort  
> - Improve decision speed & accuracy  

---

## 📝 DAX Measures List

📁 **Finance**  
- NS $ = SUM(fact_actuals_estimates[net_sales_amount])  
- NS $ LY = CALCULATE([NS $], SAMEPERIODLASTYEAR(dim_date[date]))  
- Operational Expense $ = ('Key Measures'[Ads & Promotions $]+[Other Operational Expense $])*-1  
- Other Cost $ = SUM(fact_actuals_estimates[other_cost])  
- GM $ = [NS $]-'Key Measures'[Total COGS $]  
- GM % Target = DIVIDE([GM Target $], SUM(NsGmTarget[ns_target]), 0)  
- GM % Variance = [GM % BM]-[GM %]  
- Net Profit $ = [GM $]+[Operational Expense $]  
- Net Profit % = DIVIDE([Net Profit $], [NS $], 0)  
- NP % Target = DIVIDE([NP Target $], SUM(NsGmTarget[ns_target]),0)  
- Total COGS $ = 'Key Measures'[Manufacturing Cost $] + [Freight Cost $] + 'Key Measures'[Other Cost $]  

📁 **Sales**  
- GS $ = SUM(fact_actuals_estimates[gross_sales_amount])  
- NIS $ = SUM(fact_actuals_estimates[net_invoice_sales_amount])  
- Sales Qty = CALCULATE(SUM(fact_actuals_estimates[Qty]),fact_actuals_estimates[date]<=MAX(LastSalesMonth[LastSalesMonth]))  
- Quantity = Sum(fact_actuals_estimates[Qty])  
- RC % = DIVIDE([NS $],CALCULATE([NS $],all(dim_market),all(dim_customer),all(dim_product)))  
- Market Share % = DIVIDE(SUM(marketshare[sales_$]), SUM(marketshare[total_market_sales_$]), 0)  

📁 **Marketing**  
- Customer / Product Filter Check = ISCROSSFILTERED(dim_product[product]) || ISFILTERED(dim_customer[customer])  
- Other Operational Expense $ = SUM(fact_actuals_estimates[other_operational_expense])  

📁 **Executive**  
- P & L BM = SWITCH(TRUE(), SELECTEDVALUE('Set BM'[ID])= 1, [P & L LY], SELECTEDVALUE('Set BM'[ID]) = 2, [P & L Target])  
- P & L LY = CALCULATE([P & L values], SAMEPERIODLASTYEAR(dim_date[date]))  
- Selected P & L Row = IF(HASONEVALUE('P & L Rows'[Description]),SELECTEDVALUE('P & L Rows'[Description]), "Net Sales")  

📁 **Supply Chain**  
- Net Error = [Forecast Qty]- 'Key Measures'[Sales Qty]  
- Net Error % = DIVIDE([Net Error], [Forecast Qty],0)  
- Net Error LY = CALCULATE([Net Error], SAMEPERIODLASTYEAR(dim_date[date]))  
- ABS Error % = DIVIDE('Key Measures'[ABS Error],[Forecast Qty],0)  
- ABS Error LY = CALCULATE('Key Measures'[ABS Error], SAMEPERIODLASTYEAR(dim_date[date]))  
- Forecast Accuracy % = IF('Key Measures'[ABS Error %]<>BLANK(),1-'Key Measures'[ABS Error %],BLANK())  
- Forecast Accuracy % LY = CALCULATE([Forecast Accuracy %], SAMEPERIODLASTYEAR(dim_date[date]))  
- Risk = IF([Net Error]>0, "EI", IF([Net Error]<0,"OOS", BLANK))  
- Freight Cost $ = SUM(fact_actuals_estimates[freight_cost])  
- Manufacturing Cost $ = SUM(fact_actuals_estimates[manufacturing_cost])  

📁 **Shared_UI**  
- Performance Visual Title = 'Key Measures'[Selected P & L Row] & " Performace Over Time"  
- Sales Trend Title = "NS & GM % For " & SELECTEDVALUE(dim_customer[customer])  
- Top / Bottom N title = "Top / Bottom Products & Customers by " & 'Key Measures'[Selected P & L Row]  
- Last Sales Month Footer = "Sales data loaded until : " & Format(MAX(LastSalesMonth[LastSalesMonth]), "MMM YY")  

---

## 📬 Contact

Hi, I’m **Dev Desai**, a passionate Data Analyst with a focus on **Power BI, SQL, and business storytelling**. I created this project to simulate a real-world BI solution for a mid-sized company and demonstrate my ability to turn raw data into actionable insights.

- ✉️ Email: devdesai629@gmail.com  




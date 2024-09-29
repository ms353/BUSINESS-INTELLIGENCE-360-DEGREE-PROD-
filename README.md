# BUSINESS-INTELLIGENCE-360-DEGREE-PROD-
LIVE DASHBOARD:https://app.powerbi.com/view?r=eyJrIjoiMzJmYjk3MzAtYzIwOC00NWZkLThhYTktMzNhZTYyZDIzMDExIiwidCI6IjE5MTVjZjBjLTk3YWYtNDAzOS1iMDEwLWExNGZmYzQ0ZGM0ZCJ9
PROBLEM STATEMENT:
Our case study is based on a computer hardware business which is facing challenges in dynamically changing market. Sales director decides to invest in data analysis project and would like to build power BI dashboard that can give him real time sales insights.

As sales directory of atliQ hardware has decided to invest in data analysis project he did a meeting with IT director, data analytics team to come up with a plan. We will use AIMS grid to define purpose and success criteria of this project.

Management’s Dashboard Requirements
The management wants to see these reports in the Power BI Dashboard:

Finance View — This will show the profit and loss statements across different products, markets, customers etc.

Sales View — To display the top-performing and bottom-performing customers with different key metrics.

Marketing View — Similar data as of Sales View but product based instead of customers.

Supply Chain — Reliability and accuracy to better understand the supply chain performance.

Executive View — This includes the integrated view of key insights for executives
Project Execution
Step — 1
The first step was to load the data into MySQL Database and connect it to the Power BI.

Step 2
Review and deleted the Database relationship created by Power BI by default.
Also, creating the required dimension table in Power Query.

Step — 3
Data validation using some tables in Power BI and matching the values with the data provided.

Step — 4
Data Transformation. For example, creating a Last Sales Month Reference table. So the last sales month reference table will be dynamic and will change after every sale.

Step — 5
Created calculated columns in Power Query like fiscal_year and merged the tables.

Step — 6
Data Modelling — Data modelling is a connection between different tables using a common table between them. In this project, Start Schema is used for Data Modelling where all the dimension tables were connected with Fact tables.

Step — 7
Created calculated columns using more than 40 DAX formulas (Formulas listed at the bottom). After the columns were created, verified them in either MySQL or Excel file.

Step — 8
This was the last step before start creating the design work and building the dashboards. This step was to optimize the report to reduce the report/file size. This is an important step which helps in reducing the file size so that is easy to share and access by users.

Building The Dashboard
I have created 5 different report views in this report which serves the need of various stakeholders. Let’s have a look at each of them.

The first page of the report is a home page with the navigation to all other views and a summary of each page so a user can directly access the report they need to look at.
Developed multiple feature views and dashboards to visualize key metrics and insights.
Business Terminology
Gross Price
Pre-Invoice Deductions
Post-Invoice Deductions
Net Invoice Sale
Gross Margin
Net Sales
Net Profit
COGS - Cost of Goods Sold
YTD - Year to Date
YTG - Year to Go
Direct
Retailer
Distributors
Consumer
DAX Formulas Used In This Project
ABS Error = SUMX(DISTINCT(dim_date[date]),
SUMX(DISTINCT(dim_product[product_code]), ABS([Net Error])))

ABS Error % = DIVIDE([ABS Error], [Forecast Qty], 0)

ABS Error LY = CALCULATE([ABS Error],SAMEPERIODLASTYEAR(dim_date[date]))

Ads & Promotions $ = SUM(‘fact_actuals_estimates’[ads_promotions])

Atliq MS % = CALCULATE([Market Share %], marketshare[manufacturer]=”atliq”)

BM Message = IF([NS BM $] = BLANK() || [GM % BM] = BLANK() || [NP % BM] = BLANK(), “BM Target is not available for the selected filters”, “”)

Customer / Product Filter Check = ISCROSSFILTERED(dim_product[product]) || ISFILTERED(dim_customer[customer])

Forecast Accuracy % = IF([ABS Error %]<>BLANK(), 1 — [ABS Error %], BLANK())

Forecast Accuracy % LY = CALCULATE([Forecast Accuracy %], SAMEPERIODLASTYEAR(dim_date[date]))

Forecast Qty =
var lsalesdate = MAX(LastSalesMonth[LastSalesMonth])
return
CALCULATE(SUM(fact_forecast_monthly[forecast_quantity]), fact_forecast_monthly[date]<=lsalesdate

Freight Cost $ = SUM(fact_actuals_estimates[Freight_cost])

GS $ = SUM(fact_actuals_estimates[gross_sales_amount])

Last Sales Month Home =
“Sales Data Loaded Until : ” & FORMAT(MAX(LastSalesMonth[LastSalesMonth]), “MMM YY”)

Manufacturing Cost $ = SUM(fact_actuals_estimates[manufacturing_cost])

Market Share % = DIVIDE(SUM(marketshare[sales_$]),SUM(marketshare[total_market_sales_$]), 0)

Net Error = [Forecast Qty]-[Sales Qty]

Net Error % = DIVIDE([Net Error],[Forecast Qty],0)

Net Error LY = CALCULATE([Net Error],SAMEPERIODLASTYEAR(dim_date[date]))

Net Profit % = DIVIDE([Net Profit $],[NS $],0)

Net Profit % LY = CALCULATE([Net Profit %], SAMEPERIODLASTYEAR(dim_date[date]))

Net Profit $ = [GM $]+[Operational Expense $]

NIS $ = SUM(fact_actuals_estimates[net_invoice_sales_amount])

NP % BM =
SWITCH(TRUE(),
SELECTEDVALUE(‘Set BM’[ID])=1, [Net Profit % LY],
SELECTEDVALUE(‘Set BM’[ID])=2, [NP Target %])

NP Target % = DIVIDE([NP Target $], SUM(NsGmTarget[np_target]), 0)

NP Target $ = SUM(NsGmTarget[np_target])

NS $ = SUM(fact_actuals_estimates[net_sales_amount])

NS $ LY = CALCULATE([NS $], SAMEPERIODLASTYEAR(dim_date[date]))

NS BM $ =
SWITCH(TRUE(),
SELECTEDVALUE(‘Set BM’[ID])=1,[NS $ LY],
SELECTEDVALUE(‘Set BM’[ID])=2,[NS Target $])

NS Target $ =
var tgt = SUM(NsGmTarget[ns_target])
return IF([Customer / Product Filter Check], BLANK(), tgt)

Operational Expense $ = ([Ads & Promotions $]+[Other Operational Expense $])*-1

Other Cost $ = SUM(fact_actuals_estimates[other_cost])

Other Operational Expense $ = SUM(‘fact_actuals_estimates’[other_operational_expense])

Performance Visual Title = [Selected P & L Row] & ” Performance Over Time”

Post Invoice Deduction $ = SUM(fact_actuals_estimates[post_invoice_deductions_amount])

Post Invoice Other Deduction $ = SUM(fact_actuals_estimates[post_invoice_other_deductions_amount])

Pre Invoice Deduction $ = [GS $] — [NIS $]

Quantity = SUM(fact_actuals_estimates[Qty])

RC % = DIVIDE([NS $],CALCULATE([NS $],ALL(dim_market), ALL(dim_customer), ALL(dim_product)))

Risk = IF([Net Error]>0,”EI”, IF([Net Error]<0, “OOS”, BLANK()))

Sales Qty = CALCULATE([Quantity], fact_actuals_estimates[date]<=MAX(LastSalesMonth[LastSalesMonth]))

Sales Trend Title = “NS & GM % For ” & SELECTEDVALUE(dim_customer[customer])

Selected P & L Row = IF(HASONEVALUE(‘P & L Rows’[Description]), SELECTEDVALUE(‘P & L Rows’[Description]), “Net Sales”)

Top / Bottom N Title = “Top / Bottom Products & Customers By ” & [Selected P & L Row]

Total COGS $ = ‘Key Measure’[Freight Cost $] + ‘Key Measure’[Manufacturing Cost $] + ‘Key Measure’[Other Cost $]

Total Post Invoice Deduction = ‘Key Measure’[Post Invoice Deduction $] + ‘Key Measure’[Post Invoice Other Deduction $]

post_invoice_deductions_amount =
var res = CALCULATE(MAX(post_invoice_deductions[discounts_pct]),
RELATEDTABLE(post_invoice_deductions))
return res*fact_actuals_estimates[net_invoice_sales_amount]

post_invoice_other_deductions_amount =
var res = CALCULATE(MAX(post_invoice_deductions[other_deductions_pct]),
RELATEDTABLE(post_invoice_deductions))
return res*fact_actuals_estimates[net_invoice_sales_amount]

Power Bi
Dashboard
Dax
Excel

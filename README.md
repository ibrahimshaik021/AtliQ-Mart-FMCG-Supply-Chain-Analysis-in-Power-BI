AtliQ Mart | FMCG Supply Chain Analysis in Power BI
Problem Statement
AtliQ Mart, a growing FMCG manufacturer in India, was facing significant customer churn. Several key customers were not renewing their annual contracts due to persistent delivery service issues, including orders not being delivered on time or in full. To address this and safeguard its expansion plans, management required a comprehensive analytics solution to track and improve delivery performance.

This project involved creating an interactive Power BI dashboard to provide the supply chain team with daily insights into three critical Key Performance Indicators (KPIs): On-Time Delivery (OT) %, In-Full Delivery (IF) %, and On-Time In-Full (OTIF) %.

Final Dashboard Preview
Data Model (Star Schema)
A robust star schema was implemented in Power BI to support the analysis. The model centers on the fact_orders_aggregate table for high-level KPIs, which is connected to four dimension tables: dim_customers, dim_date, dim_products, and dim_targets_orders. A secondary, more granular fact_order_lines table was used for detailed product-level analysis.

ETL Process (Power Query)
All data was provided in CSV format and was cleaned and transformed using the Power Query Editor in Power BI. Key steps in the ETL process included:

Connecting to multiple CSV source files.

Verifying and correcting data types for all 6 tables.

Ensuring clean and consistent column headers.

The data was then loaded into the Power BI model.

Key DAX Measures Created
Several DAX measures were created to power the dashboard visuals. The three primary KPIs were calculated using measures that aggregated pre-calculated flags from the fact table:

Code snippet

On_time % = DIVIDE(SUM(fact_orders_aggregate[on_time]), COUNTROWS(fact_orders_aggregate))
More complex measures were created to handle conditional counting and dynamic interactivity, such as the Line Fill Rate (LIFR) and the dynamic target line for the trend chart:

Code snippet

-- Calculates the percentage of order lines that were fulfilled completely
LIFR % =
DIVIDE(
    CALCULATE(
        COUNTROWS(fact_order_lines),
        FILTER(
            fact_order_lines,
            fact_order_lines[order_qty] = fact_order_lines[delivery_qty]
        )
    ),
    COUNTROWS(fact_order_lines)
)
Code snippet

-- Dynamically switches the target line on a chart based on a slicer selection
Dynamic Target =
SWITCH(
    TRUE(),
    SELECTEDVALUE('Metric_Selector'[Metric_Selector]) = "On_time%", [Average_ontime_%],
    SELECTEDVALUE('Metric_Selector'[Metric_Selector]) = "In_full %", [Average_infull %],
    SELECTEDVALUE('Metric_Selector'[Metric_Selector]) = "Otif %", [average_otif_%],
    BLANK()
)
Key Insights & Final Recommendation
The analysis of the completed dashboard revealed several critical insights:

Insight 1: The Problem is Systemic: The overall OTIF performance was extremely low at 29%. This poor performance was consistent across all three major cities, indicating a widespread operational issue rather than a localized one.

Insight 2: Ahmedabad is the Epicenter: Despite the widespread nature of the problem, the analysis of the customer matrix revealed that the most critical, high-volume customers with the worst OTIF performance were all concentrated in Ahmedabad.

Insight 3: Dairy is the Root Cause: A deeper analysis of the Ahmedabad-based customers showed that their order volume was overwhelmingly dominated by the Dairy product category. This strongly suggests that the root cause of the delivery failures is tied specifically to the dairy supply chain.

Final Recommendation:
Based on these insights, the key recommendation for the management team is to conduct a targeted root cause analysis of the end-to-end supply chain for Dairy products in Ahmedabad. This should include an immediate investigation into warehouse inventory levels, picking/packing processes, and last-mile delivery logistics for this specific segment to make the largest and fastest impact on improving performance and retaining key customers.

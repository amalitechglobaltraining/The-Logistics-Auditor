# Executive Summary
##### My analysis of data reveals that 89% of orders arrive on time, indicating the company is not over-promising on delivery expectations. However, late deliveries have a significant negative impact, i.e., orders delivered late receive substantially lower review scores, confirming that logistics performance directly drives customer satisfaction.
##### Geographically, five states (AL, MA, PI, SE, and BA) experience the highest late delivery rates and should be prioritized for logistics improvements.
##### When comparing product categories, Furniture shows only a marginal (~1%) higher late rate than Electronics, suggesting product type is not a major driver of delays.
##### A subset of underperforming sellers account for disproportionately high late delivery rates, presenting a clear opportunity to improve overall performance through targeted seller interventions or offboarding.

---

# Project Links
- **Dashboard:** [Tableau Public - Amalitech_Josephus](https://public.tableau.com/views/Amalitech/Amalitech_Josephus?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- **Notebook:** [Google Colab Notebook](https://colab.research.google.com/drive/1NTCVGHMe0Nb6jX37q5gbJgSo_uS-a4fS?usp=sharing)
- **Presentation:** [Google Slides Presentation](https://docs.google.com/presentation/d/1_4BFZH9irtUqOHS936DOLMaqeLC96omp/edit?usp=sharing&ouid=105911824515753931597&rtpof=true&sd=true)

---

# Technical Explanation

### Data Cleaning Approach
##### Wrangle Function: Created a reusable wrangle() function that loads CSV files while immediately displaying column names, null value percentages, and dataset shape for quick quality assessment.
##### Date Handling: Converted all date columns (order_purchase_timestamp, order_approved_at, order_delivered_carrier_date, order_delivered_customer_date, order_estimated_delivery_date) from strings to proper datetime objects using pd.to_datetime().
##### Missing Value Strategy: Orders without actual delivery dates (canceled/unavailable) were explicitly flagged as "Not Delivered" rather than dropped, preserving data integrity while excluding them from delay calculations.
##### Derived Features: Created Days_Difference (estimated minus actual delivery date) and Delivery_Status classification ("On Time", "Late", "Super Late") to enable categorical analysis.

### Candidate's Choice: Seller Performance Audit
###### Business Rationale: While geographic and product-level analysis identifies where delays occur, identifying who causes them enables direct intervention. Poor-performing sellers represent a controllable variable.
###### Methodology: Aggregated late delivery rates per seller_id, filtering for sellers with >30 orders to eliminate statistical noise from low-volume sellers.
##### Key Insight: A subset of high-volume sellers consistently underperform, making them candidates for targeted improvement programs or potential offboarding to reduce overall late delivery rates.

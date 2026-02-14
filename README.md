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

---

# Project Brief: The "Last Mile" Logistics Auditor

**Client:** Veridi Logistics (Global E-Commerce Aggregator)  
**Deliverable:** Public Dashboard, Code Notebook & Insight Presentation

---

## 1. Business Context
**Veridi Logistics** manages shipping for thousands of online sellers. Recently, the CEO has noticed a spike in negative customer reviews. She has a "gut feeling" that the problem isn't just that packages are late, but that the estimated delivery dates provided to customers are wildly inaccurate (i.e., we are over-promising and under-delivering).

She needs you to audit the delivery data to find the root cause. She specifically wants to know: **"Are we failing specific regions, or is this a nationwide problem?"**

Your job is to build a "Delivery Performance" audit tool that connects the dots between **Logistics Data** (when a package arrived) and **Customer Sentiment** (how they rated the experience).

## 2. The Data
You will use the **Olist E-Commerce Dataset**, a real commercial dataset from a Brazilian marketplace. This is a relational database dump, meaning the data is split across multiple CSV files.

* **Source:** [Kaggle - Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* **Key Files to Use:**
    * `olist_orders_dataset.csv` (The central table)
    * `olist_order_reviews_dataset.csv` (Sentiment)
    * `olist_customers_dataset.csv` (Location)
    * `olist_products_dataset.csv` (Categories)

## 3. Tooling Requirements
You have the flexibility to choose your development environment:

* **Option A (Recommended):** Use a cloud-hosted notebook like **Google Colab**, or **Deepnote**, etc.
* **Option B:** Use a local **Jupyter Notebook** or **VS Code**.
    * *Condition:* If you choose this, you must ensure your code is reproducible. Do not reference local file paths (e.g., `C:/Downloads/...`). Assume the dataset is in the same folder as your notebook.
* **Dashboarding:** The final output must be a **publicly accessible link** (e.g., Tableau Public, Google Looker Studio, Streamlit Cloud, or PowerBI Web, etc.).

---

## 4. User Stories & Acceptance Criteria

### Story 1: The Schema Builder
**As a** Data Engineer,  
**I want** to join the Orders, Reviews, and Customers tables into a single master dataset,  
**So that** I can analyze a customer's location and their review score in the same row.

* **Acceptance Criteria:**
    * Load the raw CSVs into your notebook.
    * Perform the correct joins (e.g., join Reviews to Orders on `order_id`, join Customers to Orders on `customer_id`).
    * **Check:** Ensure you don't accidentally duplicate rows (a common error with 1-to-many joins).

### Story 2: The "Real" Delay Calculator
**As a** Logistics Manager,  
**I want** to know the difference between the "Estimated Delivery Date" and the "Actual Delivery Date,"  
**So that** I can see how often we are lying to customers.

* **Acceptance Criteria:**
    * Create a new calculated column: `Days_Difference` = `order_estimated_delivery_date` - `order_delivered_customer_date`.
    * Classify orders into statuses: "On Time", "Late", and "Super Late" (> 5 days late).
    * Handle missing values: Some orders were never delivered (`order_status` = 'canceled' or 'unavailable'). These should be excluded or flagged separately.

### Story 3: The Geographic Heatmap
**As a** Regional Director,  
**I want** to see which specific States (`customer_state`) have the highest percentage of late deliveries,  
**So that** I can focus my repair efforts on the worst regions.

* **Acceptance Criteria:**
    * Calculate the % of late orders per State.
    * Visualize this on a map or a bar chart.
    * **Insight:** Identify if "Remote" states (far from the distribution center) are disproportionately affected.

### Story 4: The Sentiment Correlation
**As a** Customer Success Lead,  
**I want** to see if late deliveries actually cause bad reviews,  
**So that** I can prove to the CEO that logistics is the problem.

* **Acceptance Criteria:**
    * Create a visualization comparing "Delivery Delay (Days)" vs "Average Review Score (1-5)".
    * Show the average review score for "On Time" orders vs. "Late" orders.

---

## 5. Bonus User Story: The "Translation" Challenge
**As a** Global Analyst,  
**I want** to see product categories in **English**, not Portuguese,  
**So that** I can understand if "Furniture" is harder to ship than "Electronics".

* **Acceptance Criteria:**
    * The `product_category_name` is in Portuguese (e.g., `cama_mesa_banho`).
    * Use the `product_category_name_translation.csv` file included in the dataset (or create your own mapping) to translate these into English for your final dashboard.

---

## 6. The "Candidate's Choice" Challenge
**As a** Creative Problem Solver,  
**I want** to include one extra feature or analysis that adds specific business value,  
**So that** I can demonstrate my ability to think beyond the basic requirements.

* **Instructions:**
    * Add one more metric, chart, or drill-down.
    * **Requirement:** You must justify *why* this feature matters to the business in your README.

---

## 7. Submission Guidelines
Please edit this `README.md` file in your forked repository to include the following three sections at the top:

### A. The Executive Summary
* A 3-5 sentence summary of your findings.

### B. Project Links
* **Link to Notebook:** (e.g., Google Colab, etc.). *Ensure sharing permissions are set to "Anyone with the link can view".*
* **Link to Dashboard:** (e.g., Tableau Public, etc.).
* **Link to Presentation:** A link to a short slide deck (PDF/PPT) AND (Optional) a 2-minute video walkthrough (YouTube) explaining your results.

### C. Technical Explanation
* Briefly explain how you handled the "Data Cleaning".
* Explain your "Candidate's Choice" addition.

**Important Note on Code Submission:**
* Upload your `.ipynb` notebook file to the repo.
* **Crucial:** Also upload an **HTML or PDF export** of your notebook so we can see your charts even if GitHub fails to render the notebook code.
* Once you are ready, please fill out the [Official Submission Form Here](https://forms.office.com/e/heitZ9PP7y) with your links

---

## 🛑 CRITICAL: Pre-Submission Checklist

**Before you submit your form, you MUST complete this checklist.**

> ⚠️ **WARNING:** If you miss any of these items, your submission will be flagged as "Incomplete" and you will **NOT** be invited to an interview. 
>
> **We do not accept "permission error" excuses. Test your links in Incognito Mode.**

### 1. Repository & Code Checks
- [ ] **My GitHub Repo is Public.** (Open the link in a Private/Incognito window to verify).
- [ ] **I have uploaded the `.ipynb` notebook file.**
- [ ] **I have ALSO uploaded an HTML or PDF export** of the notebook.
- [ ] **I have NOT uploaded the massive raw dataset.** (Use `.gitignore` or just don't commit the CSV).
- [ ] **My code uses Relative Paths.** 

### 2. Deliverable Checks
- [ ] **My Dashboard link is publicly accessible.** (No login required).
- [ ] **My Presentation link is publicly accessible.** (Permissions set to "Anyone with the link can view").
- [ ] **I have updated this `README.md` file** with my Executive Summary and technical notes.

### 3. Completeness
- [ ] I have completed **User Stories 1-4**.
- [ ] I have completed the **"Candidate's Choice"** challenge and explained it in the README.

**✅ Only when you have checked every box above, proceed to the submission form.**

---

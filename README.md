
# Hospital Supply Chain Analysis Summary

## 1. Objective of the Analysis

The primary goal of this project was to analyze the hospital’s supply chain by linking inventory data and patient records to:

- **Monitor Supply Management:** Track restocking, usage rates, and shortages.
- **Assess Supplier Performance:** Evaluate suppliers’ reliability and delivery lead times.
- **Link Patient Care to Supply Use:** Check how patient admissions drive inventory consumption.
- **Identify Critical Shortages:** Highlight high-demand supplies and potential risks.
- **Optimize Resource Utilization:** Examine room types, staff needs, and procedures performed.

---

## 2. Process Overview

### A. Data Loading & SQL Queries

We used two CSV files:  
- **`inventory_data.csv`** – Contains stock levels, costs, and supplier info.  
- **`patient_data.csv`** – Includes patient admissions, procedures, and supplies used.

**Key SQL Queries Executed:**

1. **Most Frequently Restocked Items:**  
Identified items with the highest restocking frequency using:

```sql
SELECT Item_Name, COUNT(*) AS Restock_Count
FROM inventory_data
WHERE Current_Stock < Min_Required
GROUP BY Item_Name
ORDER BY Restock_Count DESC;
```

2. **Supplier Performance:**  
Checked suppliers with the most restocks and shortest lead times:

```sql
SELECT Vendor_ID, COUNT(*) AS Restock_Count, AVG(Restock_Lead_Time) AS Avg_Lead_Time
FROM inventory_data
WHERE Current_Stock < Min_Required
GROUP BY Vendor_ID
ORDER BY Restock_Count DESC, Avg_Lead_Time ASC;
```

3. **Linking Supplies to Patient Needs:**  
Matched patient procedures to supply stock levels to see if patient care caused shortages:

```sql
SELECT p.Admission_Date, p.Primary_Diagnosis, p.Supplies_Used, i.Item_Name, 
       i.Current_Stock, i.Min_Required, (i.Min_Required - i.Current_Stock) AS Stock_Deficit
FROM patient_data p
JOIN inventory_data i ON p.Supplies_Used LIKE '%' || i.Item_Name || '%'
WHERE i.Current_Stock < i.Min_Required
ORDER BY p.Admission_Date, Stock_Deficit DESC;
```

4. **Critical Shortages:**  
Identified items with the worst stock deficits during peak patient admissions.

---

### B. Exporting SQL Query Results  

- Saved all query results to CSV files.  
- Combined these results into a multi-sheet Excel file for easier analysis and visualization.

---

### C. Data Visualization in Excel  

#### 1. Loaded Data into Excel:
- Opened the exported Excel file containing multiple sheets.  
- Each sheet represented a query result: restocks, shortages, supplier performance, and patient care links.

#### 2. Created Visualizations:

**Dashboards Built:**

- **Inventory Management:**
  - **Bar Chart:** Top 10 most frequently restocked items.
  - **Line Chart:** Stock deficit trends over time.
  - **Table:** Supplier performance based on lead time and restock frequency.

- **Patient Care Insights:**
  - **Line Chart:** Monthly patient admissions.
  - **Heatmap:** Supplies linked to medical procedures.

- **Resource Utilization:**
  - **Stacked Bar Chart:** Room type usage and staff allocation trends.

---

## 3. Findings & Insights  

### A. Inventory & Restocking
- **Top Restocked Items:**  
  Surgical masks, IV drips, and gloves were frequently restocked due to high daily usage.
- **Most Expensive Supplies:**  
  Equipment like ventilators and MRI machines contributed the most to overall expenses.

### B. Supplier Performance
- **Reliable Vendors:**  
  Suppliers with shorter restock lead times had higher reliability but higher associated costs.
- **Delays Identified:**  
  Certain items experienced long restock delays, creating risks of stockouts.

### C. Patient Care & Critical Shortages
- **Direct Supply-Patient Link:**  
  Patient admissions for procedures like appendectomies and trauma care drove supply depletion.
- **Peak Demand Periods:**  
  Monthly admissions peaked during specific months, leading to predictable supply shortages.

### D. Resource Utilization
- **Room & Staff Utilization:**  
  ICU rooms required the highest staff levels and had the longest average patient stays.

---

## 4. Conclusion & Recommendations  

Based on the analysis, the following steps could improve hospital supply chain efficiency:

1. **Automate Inventory Management:**  
   - Use predictive models for restocking based on patient admission trends.

2. **Supplier Contract Review:**  
   - Consider alternative vendors for supplies with long restock lead times.

3. **Stock Buffering:**  
   - Increase safety stock levels for high-demand items like PPE and critical care equipment.

4. **Data-Driven Decision Making:**  
   - Integrate dashboards into hospital operations to enable real-time monitoring and forecasting.

---


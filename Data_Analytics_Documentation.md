 **ELECTRIC POPULATION DATA ANALYTICS DOCUMENTATION**

1. **PROJECT OVERVIEW**

This activity focused on applying descriptive analytics to Electric Vehicle data to identify trends and generate meaningful summaries. The entire data workflow was carried out in Power BI, starting with data preprocessing where the dataset was cleaned, standardized, and checked for consistency to ensure accuracy. 

This activity focused on applying descriptive analytics to Electric Vehicle (EV) data to identify trends and summarize key insights. The entire process was done in Power BI, starting with data preprocessing to clean and standardize the dataset.

Data modeling was then performed by creating fact and dimension tables using a star schema, along with DAX measures such as total BEV, total PHEV, and average electric range. Exploratory data analysis (EDA) was conducted to examine distributions and patterns in EV types, manufacturers, and electric range.

Finally, an interactive Power BI dashboard was created to visualize the results using charts, slicers, and filters, allowing dynamic exploration of the data.

**2\. DATA COLLECTION PROCEDURE**

## **Raw Dataset Profile**

| Attribute | Description |
| ----- | ----- |
| Dataset Name | Electric Vehicle Population Data |
| File Format | CSV |
| Tools Used | Microsoft Excel and Power BI |
| Dataset Type | Structured Dataset |
| Analysis Type | Descriptive Analytics |
| Number of Rows | 135,038 rows |
| Number of Columns | 17 columns |

## **Dataset Description**

The dataset contains information related to electric vehicle registrations and characteristics. The dataset includes vehicle manufacturers, model years, electric vehicle classifications, electric ranges, geographic locations, and utility providers.

The dataset originally consisted of **(135,038)** records and **(17)** columns before preprocessing and transformation.

**3\. DATA CLEANING PROCESS/DOCUMENTATION**  
The dataset underwent data cleaning and preprocessing to improve data quality, consistency, completeness, and analytical reliability before visualization and dashboard development.

The following issues were identified and resolved during preprocessing:

| Table | Column | Issue | Row Count | Magnitude | Solvable? | Resolution |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Fact\_EV\_Data | Electric Range | Missing numeric values | 1 | 0.00% | Yes | Replaced null values with 0 |
| Fact\_EV\_Data | Postal Code | Missing categorical values | 8 | 0.01% | Yes | Replaced blank values with “Unknown” |
| Fact\_EV\_Data | County | Blank rows | 8 | 0.01% | Yes | Replaced blank values with “Unknown” |
| Fact\_EV\_Data | City | Blank rows | 8 | 0.01% | Yes | Replaced blank values with “Unknown” |
| Fact\_EV\_Data | Electric Utility | Blank rows | 8 | 0.01% | Yes | Replaced blank values with “Unknown” |
| Fact\_EV\_Data | VIN (1-10) | No analytical value | 135,038 | 100.00% | Yes | Removed during preprocessing |
| Fact\_EV\_Data | Vehicle Location | Redundant geographic data | 135,038 | 100.00% | Yes | Removed during preprocessing |
| Fact\_EV\_Data | 2020 Census Tract | Data not needed for business analytics | 135,038 | 100.00% | Yes | Removed during preprocessing |
| Fact\_EV\_Data | Postal Code | Wrong data type causing formatting issues and loss of leading zeroes | 135,038 | 100.00% | Yes | Converted data type from Whole Number to Text |
| dim\_vehicle | Make | Duplicate categorical values from fact table extraction leading to inconsistent dimension relationships | 36 | 0.03% | Yes | Removed duplicates to ensure unique manufacturer values |
| dim\_location | County | Repeated geographic values inherited from transactional data leading to inconsistent relationship mapping | 170 | 0.13% | Yes | Removed duplicates to ensure unique county values |
| dim\_utility | Electric Utility | Duplicate utility entries caused by multiple fact-level records leading to inconsistent lookup relationships | 77 | 0.06% | Yes | Removed duplicates to ensure unique utility values |
| dim\_date | Model Year | Repeated time values from transactional data leading to inconsistent time-based relationships | 22 | 0.02% | Yes | Removed duplicates to ensure unique model years |

1. # **Handling Missing Values**

| Column Type | Replacement |
| ----- | ----- |
| Numerical Fields | 0 |
| Text Fields | Unknown |

Purpose:  
Missing values were handled to maintain data completeness and prevent analytical errors during visualization and reporting.  
---

2. # **Duplicate Handling**

Duplicate records were removed using the DOL Vehicle ID column because it serves as the unique identifier for each vehicle record.  
---

3. # **Data Transformation**

Additional calculated columns were created to improve analysis and reporting.  
---

## **c.1 Vehicle Age**

Formula:  
Vehicle Age \= 2026 \- Model Year  
Purpose:  
The Vehicle Age column was created to analyze the age distribution of electric vehicles.  
---

## **c.2 Range Category**

| Electric Range | Category |
| ----- | ----- |
| 0–100 | Low Range |
| 101–250 | Medium Range |
| Above 250 | High Range |

Purpose:  
Range Category was created to simplify battery range analysis through grouped categories.  
---

## **c.3 Adoption Period**

| Model Year | Adoption Period |
| ----- | ----- |
| Before 2015 | Early Adoption |
| 2015–2020 | Growth Period |
| After 2020 | Modern Adoption |

Purpose:  
Adoption Period was created to analyze the phases of electric vehicle adoption over time.

4\. **DATA MODEL**

## **Star Schema Model**

A Star Schema model was implemented to simplify data relationships and improve dashboard performance and analytical efficiency.

# **FACT TABLE**

## **Fact\_EV\_Data**

The Fact Table contains the main measurable and transactional electric vehicle records.

### **Fact Table Columns**

* DOL Vehicle ID  
* Electric Range  
* Model Year  
* County  
* City  
* State  
* Postal Code  
* Make  
* Model  
* Electric Vehicle Type  
* Electric Utility  
* Vehicle Age  
* Range Category  
* Adoption Period

---

# **DIMENSION TABLES**

## **dim\_vehicle**

| Column | Purpose |
| ----- | ----- |
| Make | Used for manufacturer filtering and vehicle analysis |

---

## **dim\_location**

| Column | Purpose |
| ----- | ----- |
| County | Used for geographic and regional analysis |

---

## **dim\_date**

| Column | Purpose |
| ----- | ----- |
| Model Year | Used for time-series and trend analysis |

---

## **dim\_utility**

| Column | Purpose |
| ----- | ----- |
| Electric Utility | Used for utility provider analysis |

---

# **RELATIONSHIPS**

| Fact Table Column | Dimension Table Column | Relationship |
| ----- | ----- | ----- |
| Make | dim\_vehicle\[Make\] | One-to-Many |
| County | dim\_location\[County\] | One-to-Many |
| Model Year | dim\_date\[Model Year\] | One-to-Many |
| Electric Utility | dim\_utility\[Electric Utility\] | One-to-Many |

**5\. DASHBOARD WIREFRAME LAYOUT**

**6\. VISUALIZATION & DASHBOARD**

| KPI Card | Purpose |
| ----- | ----- |
| Total Vehicles | Displays the total number of electric vehicle records in the dataset |
| Average EV Range | Displays the average electric driving range of vehicles |
| Total BEV | Displays the total number of Battery Electric Vehicles |
| Total PHEV | Displays the total number of Plug-in Hybrid Electric Vehicles |

| Slicer | Purpose |
| ----- | ----- |
| Make | Filters dashboard visualizations based on vehicle manufacturer |
| County | Filters visualizations according to geographic county location |
| Model Year | Filters data based on vehicle model year |
| Electric Vehicle Type | Filters dashboard based on EV classification such as BEV or PHEV |
| Range Category | Filters vehicles according to battery range classification |

| Visualization | Chart Type | Purpose |
| ----- | ----- | ----- |
| Total Vehicles by Model Year | Line Chart | Shows electric vehicle growth and adoption trends over time |
| Total Vehicles by Electric Vehicle Type | Donut Chart | Shows proportional distribution of Battery Electric Vehicles (BEV) and Plug-in Hybrid Electric Vehicles (PHEV) |
| Total Vehicles by Make | Stacked Bar Chart | Identifies the most common and leading electric vehicle manufacturers |
| Total Vehicles by Range Category | Stacked Column Chart | Shows the distribution of electric vehicles based on battery range categories |
| Total Vehicles by County | Clustered Bar Chart | Displays counties with the highest electric vehicle adoption rates |

**6\. INSIGHT AND RECOMMENDATION**

| INSIGHT 1 Battery Electric Vehicles (BEVs) dominate the dataset The dashboard shows that Battery Electric Vehicles (BEVs) have a larger population compared to Plug-in Hybrid Electric Vehicles (PHEVs). This indicates that more consumers are shifting toward fully electric transportation instead of hybrid alternatives. Recommendation:Government agencies and private companies should expand EV charging infrastructures such as public charging stations in malls, highways, and business centers to support the increasing number of BEV users. |
| :---- |
| **INSIGHT 2 Average EV range influences consumer preference** The Average EV Range metric shows that vehicles with longer driving ranges are more attractive to users because they reduce charging frequency and “range anxiety.” **Recommendation:** Manufacturers should focus on improving battery efficiency and range capacity to make EVs more practical for long-distance travel. |
| **INSIGHT 3 EV growth supports environmental sustainability** The increasing number of electric vehicles contributes to lower greenhouse gas emissions and reduced dependence on traditional fuel-powered transportation. **Recommendation:**Governments and environmental organizations should continue supporting clean energy initiatives and renewable energy integration for EV charging systems. |


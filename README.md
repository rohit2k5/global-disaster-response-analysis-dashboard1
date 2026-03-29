###🌍 Global Disaster Analytics Dashboard (Power BI)
📌 Project Summary
This project presents a Global Disaster Analytics Dashboard built using Power BI to analyze worldwide disaster data from 2018 to 2024.
It provides insights into:
•	Disaster frequency and trends
•	Economic impact and casualties
•	Response efficiency and recovery duration
•	Aid and funding distribution
•	Forecasting future disaster patterns
The dashboard is designed to support data-driven decision-making for disaster management and planning.
________________________________________
📂 Dataset Source
•	Dataset: Global Disaster Data (2018–2024)
•	Type: Structured dataset (CSV format)
•	Contains:
o	Event ID
o	Disaster Type
o	Country & Region
o	Economic Loss (USD)
o	Casualties
o	Response Time (days)
o	Recovery Duration (days)
Note: A sample dataset is included in this repository. Full dataset may be excluded due to size limitations.
________________________________________
⚙️ How to Run ETL Pipeline
Step 1: Install Requirements
pip install -r requirements.txt
Step 2: Run ETL Scripts
python src/etl/01_load.py
python src/etl/02_clean.py
python src/etl/03_enrich.py
ETL Workflow:
•	01_load.py → Loads raw dataset
•	02_clean.py → Handles missing values, formats data
•	03_enrich.py → Adds calculated fields and features
Processed data will be saved in:
data/clean/
________________________________________
📊 How to Open Power BI Dashboard
1.	Go to:
powerbi/GlobalDisasterDashboard.pbix
2.	Open using:
•	Power BI Desktop
3.	Interact with:
•	Filters (Year, Country, Disaster Type)
•	Visuals (Maps, Charts, KPIs)
________________________________________

📷 Dashboard Screenshots
Executive Overview
  
Regional Analysis
  



Response Performance
  
Aid & Funding
 
Recovery & Forecasting  
________________________________________
🛠️ Tools & Technologies
•	Power BI
•	DAX (Data Analysis Expressions)
•	Python (ETL Pipeline)
•	Pandas, NumPy
•	Git & GitHub
________________________________________
📁 Project Structure
global-disaster-dashboard/
global-disaster-dashboard/
├── README.md
├── LICENSE
├── data/
│   ├── raw/
│   │   └── disasters_raw_2018_2024.csv
│   ├── clean/
│   │   └── disasters_clean_2018_2024.parquet
│   └── docs/
│       └── data_dictionary.md
├── docs/
│   └── dashboard_blueprint.png
├── src/
│   ├── etl/
│   │   ├── 01_load.py
│   │   ├── 02_clean.py
│   │   └── 03_enrich.py
│   ├── analysis/
│   │   └── eda_notebook.ipynb
│   └── utils/
│       └── country_mapping.json
├── powerbi/
│   ├── GlobalDisasterDashboard.pbix
│   └── DAX_measures.txt
├── notebooks/
│   └── forecasting_example.ipynb
├── tests/
│   └── test_data_quality.py
├── requirements.txt
└── .github/
    └── workflows/
        └── ci.yml
________________________________________
🧠 Key Insights
1.	Insight: “Country A accounts for 28% of total economic loss from 2018–2024 but only 10% of total aid.”
o	Interpretation: Aid distribution is not proportional to economic impact.
o	Recommendation: Investigate barriers to aid delivery to Country A; re-balance aid channels.
2.	Insight: “Average response time in Region X is 96 hours vs global average 48 hours; recovery duration is 1.9× longer.”
o	Interpretation: Slower responses correlate with longer recovery.
o	Recommendation: Establish regional response hubs, pre-position supplies to cut response time.
3.	Insight: “Floods cause 40% of incidents but wildfires have 3× the average economic loss per event.”
o	Interpretation: Frequency ≠ severity; resource allocation should consider per-event impact as well as frequency.
o	Recommendation: Reallocate specialized firefighting and insurance resources.
4.	Insight: “Events with aid > $1M have 15% higher efficiency score and 20% shorter recovery on average.”
o	Interpretation: Larger aid amounts improve operational performance, but causality needs careful testing.
o	Recommendation: Fast-track funding approvals for major incidents; monitor utilization.
5.	Insight: “Top 5 donors supply 70% of aid, but aid-per-capita in several affected countries remains low.”
o	Interpretation: Aid concentration among donors may miss vulnerable small countries.
o	Recommendation: Set aside a proportional emergency fund for low-aid countries.
6.	Insight: “Response time shows high variance for earthquake events (large boxplot spread).”
o	Interpretation: Earthquake response protocols may be inconsistent.
o	Recommendation: Standardize earthquake rapid response protocols across responders.
7.	Insight: “Recent years show a rising trend in disaster frequency in Coastal Regions, with increasing economic loss.”
o	Interpretation: Possibly climate-driven trend; need adaptation investment.
o	Recommendation: Prioritize coastal resilience programs and early warning systems.
8.	Insight: “High efficiency score correlates to shorter recovery only up to a point; beyond very high aid amounts, marginal recovery improvement diminishes.”
o	Interpretation: Diminishing returns on aid; efficiency matters as much as volume.
o	Recommendation: Focus on allocation efficiency, capacity building, and transparency.
________________________________________
🧪 Data Quality Checks
•	Data types & parsing
o	Ensure EventID is unique and text/ID typed.
o	Parse EventDate into date data type; extract Year, Month, Quarter columns.
o	Ensure numeric fields (Casualties, EconomicLoss, AidAmount, ResponseTimeHours, RecoveryDurationDays, EfficiencyScore) are numeric.
•	Missing values
o	Identify NULLs per column. Strategy:
	If EventID, EventDate, or Country missing — drop or flag as incomplete.
	If Casualties or EconomicLoss missing — impute as 0 only if confirmed; better: flag and treat separately in visuals.
	If ResponseTimeHours missing — do not include in averages unless imputed; create a flag ResponseTimeKnown.
•	Duplicates
o	Check for duplicate EventID or same (EventDate, Country, Location, DisasterType). Remove exact duplicates.
•	Standardize categorical fields
o	Normalize DisasterType (e.g., floods, flood, Flood -> Flood).
o	Normalize Country names (use ISO country names or codes).
o	Create Region mapping (continent/UN region) for each country.
•	Units & currency
o	Ensure EconomicLoss and AidAmount use same currency & units (e.g., USD). If mixed, convert to single currency; adjust for scale (thousands, millions).
o	If comparing across years, consider inflation-adjusted economic loss (adjust to constant year USD).
•	Outliers & validation
o	Flag extreme values (e.g., economic loss beyond reasonable range). Validate against external sources if possible.
o	Ensure EfficiencyScore in expected range (e.g., 0–100). Clip/flag out-of-range values.
•	Date consistency & ranges
o	Ensure event dates fall between 2018-01-01 and 2024-12-31. Flag events outside.
•	Geographic enrichment
o	Add latitude/longitude for mapping if available; otherwise add centroid coordinates by country for choropleth/bubble map.
•	Create calculated fields
o	SeverityIndex (composite score): normalized weighted sum of casualties, economic loss, and severity rating (if exists).
o	Year, Month, Quarter, WeekOfYear.
o	ResponseTimeKnown boolean, HasAid boolean, IsMajor (SeverityIndex threshold).
•	Audit trail
o	Add Source and IngestDate columns for traceability.
o	Keep raw original file in data/raw/ and processed file in data/clean/.
•	
________________________________________
🚀 Future Improvements
•	Real-time data integration
•	Advanced machine learning forecasting
•	Deployment on Power BI Service
•	API-based data ingestion
________________________________________
👨‍💻 Author
Rohit Mirage
Data Analyst
________________________________________
⭐ Support
If you found this project useful, consider giving it a ⭐ on GitHub!


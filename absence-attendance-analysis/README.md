# Employee Absence & Attendance Analysis

Business Problem: Employee Absence
Northstar Distribution Group is a large UK organisation employing approximately 2,500 people across multiple departments, locations and shift patterns.
Senior leadership has noticed that employee absence has become a growing operational concern. Absence levels appear to vary considerably between departments, locations and working patterns, but the organisation currently lacks a clear, centralised view of the data.
The existing absence information comes from several different systems and contains inconsistencies, making it difficult for leadership to determine:
•	How much absence the business is experiencing
•	Whether absence is increasing or decreasing
•	Which departments and locations are most affected
•	Which absence reasons are driving the greatest number of days lost
•	Whether short-term or long-term absence is the bigger issue
•	Whether particular shifts or employee groups have higher absence
•	Whether there are recurring patterns of absence
•	Where management intervention could have the greatest impact
Project Objective – What are we trying to achieve? 
To analyse 12 months of employee absence data across Northstar Distribution Group, identify key trends and patterns, and provide leadership with actionable insights into the scale, causes and distribution of absence.
The project will involve data-quality assessment and cleansing using SQL, analytical modelling and visualisation using Power BI, and the development of evidence-based recommendations.
Data Quality – What needs cleansing before we start?
Imported my data by creating a table for each existing CSV I have. Utilised BULK INSERT to import the data from the CSV files. Recognised a date type mismatch for 3 columns so amended these utilising ALTER COLUMN and completed the import for absences. StartDate and EndDate were incorrectly added as data type DATE, this was amended to DATETIME. DurationDays was also listed as an INT but this was amended to DECIMAL considering that not all days recorded were full working days. 
Reviewing the Tables
For each table I am reviewing; 
-	Unnecessary duplicates
-	NULL values
-	Incorrect spelling
-	Hidden characters
-	Data Type relevance
Absence Reasons: 
-	Reason ID set as Primary Key, not nullable and all values are unique. 
Absences: 
-	Absence ID set as Primary Key, not nullable and all values are unique. 
-	There are 20 missing EndDate values. Where StartDate and DurationDays are present, an end date may be derived—but first verify the rule against complete records. If dates are inclusive calendar days, the usual calculation is StartDate + DurationDays - 1. Missing end dates could also mean an absence is still open.
-	There are 15 missing DurationDays values. Where both dates are present, duration can be derived using the confirmed date rule. Do not derive it where either date is also missing.
-	Status has inconsistent capitalisation, such as approved and Approved. Standardise it to one format, e.g. Approved, so reporting groups them together. Also check for leading/trailing spaces.
-	StartDate and EndDate can be converted from date-time to DATE if the time component is always 00:00:00 and no time-of-day information is needed.
-	All EmployeeID, ShiftID, and ReasonID values match a record in their related tables. This is a positive: there are no orphaned relationships. It’s also worth confirming none of these key columns contain NULL values.
Calendar: 
-	C_Date ID set as Primary Key, not nullable and all values are unique. 
-	C_Date can be changed from DATETIME to DATE, as no time-of-day information is used.
Departments: 
-	Department ID set as Primary Key, not nullable and all values are unique. 
Locations: 
-	Location ID set as Primary Key, not nullable and all values are unique. 
Shifts: 
-	Shift ID set as Primary Key, not nullable and all values are unique.
-	Start time and End time can be made simpler as they are currently seconds but the fractional seconds are not used. 
Employee Monthly Snapshot: 
-	240 of the Employment Type Cells are NULL. 
-	There are two versions of each LocationID as some of them have a blank space in front of the characters, this needs to be trimmed. This is interfering with grouping the data. 
Employees: 
-	Employee ID set as Primary Key, not nullable and all values are unique.
-	Date of birth and Hire Date can be simplified from a Date Time type to a Date type as the time is not required here.
-	Some of the department ids are capitalised and some are not, this is not interfering with the grouping but will be amended for standardisation. 
-	There are two versions of each locationID as some of them have a blank space in front of the characters, this needs to be trimmed. This is interfering with grouping the data. 
-	12 of the DateofBirth cells are showing as NULL
-	10 of the HireDate cells are showing as NULL
-	20 of the EmploymentTypes are showing as NULL
Data Cleansing and Transformation – What actions have been taken to prepare the data?
Data Cleansing
Absences
•	Missing EndDate values: Updated missing values using StartDate and DurationDays, applying the duration rule verified against existing records.
•	Missing DurationDays values: Updated missing values using StartDate and EndDate, following the established duration calculation.
•	Status inconsistency: Standardised status values by changing lowercase and whitespace-affected versions of approved to Approved. The field now contains two consistent categories.
•	Date data types: Changed StartDate and EndDate from DATETIME to DATE, as time-of-day information was not required.
Calendar
•	Date data type: Changed C_Date from DATETIME to DATE, as time-of-day information was not used.
Shifts
•	Time format: Changed the shift start and end time fields to TIME(0), removing fractional seconds and retaining values in HH:MM:SS format.
Employees
•	Date data types: Changed DateOfBirth and HireDate from DATETIME to DATE, as time-of-day information was not required.
•	Department ID inconsistency: Standardised DepartmentID values using uppercase formatting.
•	Location ID duplicates: Removed leading and trailing spaces from LocationID values to prevent duplicate-looking IDs and improve grouping accuracy.
•	Missing values: DateOfBirth, HireDate, and EmploymentType missing values remain as NULL, as no reliable source was available to populate them.
Initial Transformations
To prepare the dataset for analysis and ensure a robust data model, several transformations were carried out within Power BI:
1.	Data Import and Model Setup The absence and employee datasets were imported into Power BI, forming the foundation for the data model. The absences table was identified as the primary fact table, with supporting employee attributes acting as dimension data in a star schema structure.
2.	Merging Department and Location Information DepartmentID and LocationID were merged from the employees table into the absences table. This allowed direct relationships to be established between absence records and organisational structures (department and location), ensuring accurate filtering and aggregation within visuals.
3.	Creation of Additional Employee Attributes Using the existing HireDate and DateOfBirth fields, four new columns were created:
o	Tenure (years)
o	Tenure Group
o	Age
o	Age Group These transformations were introduced to enable more flexible analysis of workforce characteristics and to support potential segmentation in future iterations of the dashboard.
4.	Removal of Non Relevant Absence Records Absence entries with a status of “Cancelled” were removed from the dataset. These records do not represent actual absence events and would otherwise distort totals, averages, and trend analysis.
Data Analysis: What methodology are we utilising
I decided to utilise an Exploratory Data Analysis utilising PowerBI. On the back of this I would build a dashboard that is designed to be accessed by leaders of different departments and locations to view the ongoing trends from the data. 
DAX Measures
A series of DAX measures were created to support KPI calculations, benchmarking, and dynamic filtering within visuals:
•	TotalExpectedDays = SUM(employees[Daysperyear]) Used to estimate the total number of working days per employee (after deducting 32 days of holiday allowance). This forms the basis for calculating expected working time across the business.
•	TotalAbsence = SUM(absences[DurationDays]) Calculates the total number of absence days recorded.
•	AbsenceRate = [TotalAbsence] / [TotalExpectedDays] Determines the percentage of absence days relative to expected working days.
•	AverageDays = AVERAGE(absences[DurationDays]) Provides the average duration of absence instances.
•	AbsenceRecordCount = DISTINCTCOUNT(absences[AbsenceID]) Counts the number of absence events.
•	TotalEmployees = DISTINCTCOUNT(employees[EmployeeID]) Returns the total number of employees within the selected area.
•	NoOfEmployees = DISTINCTCOUNT(absences[EmployeeID]) Identifies how many employees have recorded at least one absence.
•	AverageAbs = [TotalAbsence] / [TotalEmployees] Calculates average absence days per employee, allowing fair comparison between departments of different sizes.
•	Absence % of Selected Total = DIVIDE( [TotalAbsence],CALCULATE( [TotalAbsence], ALLSELECTED(absences), REMOVEFILTERS(absences[EmployeeID]): Shows the percentage contribution of an individual employee’s absence relative to the total absence within the selected filters (e.g., department, location). 
•	All Average Absence = DIVIDE(CALCULATE([TotalAbsence], REMOVEFILTERS()), CALCULATE([TotalEmployees], REMOVEFILTERS()),0): Provides the organisation wide average absence, used as a benchmark for comparison.
•	Comparison Label = VAR Difference = [AverageAbs] - [All Average Absence]RETURNIF(Difference > 0,"+" & FORMAT(Difference, "0.00") & " days above the company average", IF(Difference < 0,FORMAT(ABS(Difference), "0.00") &" days below the company average","Equal to the company average" )): Generates a dynamic text label indicating whether a selected department/location is above or below the company average.
•	Rank = RANKX(ALL('absences'[EmployeeID]), [TotalAbsence], ,DESC,Dense): Ranks employees by total absence, enabling Top 5 analysis and highlighting high absence individuals.
Data Visualisation: How are we going to present this data?
Data Visualisation Approach
To communicate insights clearly and support decision making, I designed a set of visuals in Power BI that present absence trends, patterns, and comparisons in an intuitive and accessible format. The visualisation approach focused on clarity, interactivity, and alignment with the needs of departmental and location based leaders.
Dashboard Structure
The dashboard is organised into a headline overview section followed by detailed breakdown visuals. This structure allows users to quickly understand overall absence levels before exploring specific drivers or areas of concern.
Key Visualisation Choices
•	KPI Cards Used to present core metrics such as total absence days, number of absence records, average duration, absence rate, and number of employees with absence. KPI cards provide immediate, high level insight and act as entry points for deeper analysis.
•	Line Chart (Absence Trend Over Time) A 12 month line chart was used to show how absence days fluctuate month to month. This helps identify seasonal patterns, spikes, or improvements over time.
•	Donut Chart (Absence Category Breakdown) The split between short term and long term absence is shown using a donut chart, offering a clear proportional view of absence types.
•	Pie Chart (Top 5 Absence Reasons) A pie chart highlights the most common reasons for absence, enabling leaders to quickly identify key drivers such as cold/flu, mental health, or musculoskeletal issues.
•	Interactive Slicers Slicers for department, location, and employment type allow users to filter the dashboard dynamically. This supports targeted analysis and ensures the visuals respond to organisational structure.
•	Top 5 Employee Table A ranked table displays employees with the highest absence levels, supported by DAX measures that calculate contribution percentages and dynamic ranking. This helps identify individuals with significant absence impact.
•	Tooltip Pages Custom tooltip pages were added to provide additional context when hovering over employees, including profile information and absence contribution metrics. This enhances interactivity without cluttering the main dashboard.
Outcome
The final dashboard provides a clear, interactive, and insightful view of absence trends across the organisation. Leaders can quickly identify patterns, compare departments or locations, and understand the drivers behind absence levels.
 
 

Recommendations: What actions could we recommend based on the findings? 
Based on the analysis of absence trends, reasons, and departmental/location patterns, several targeted actions can be recommended to help reduce absence levels and support employee wellbeing.
1. Address High Levels of Cold and Flu Absence
Cold and flu are the largest contributors to overall absence. Recommended actions:
•	Provide free flu vouchers or on site flu vaccination clinics.
•	Increase communication around seasonal illness prevention (e.g., hygiene, staying home when unwell).
•	Consider flexible working options during peak illness periods.
2. Strengthen Mental Health Support
Mental health is a major driver of absence in several locations, particularly Derby. Recommended actions:
•	Promote existing mental health resources such as the Employee Assistance Programme.
•	Offer manager training on mental health conversations and early intervention.
•	Increase visibility of wellbeing initiatives and support channels.
3. Implement Policies for Persistent Short Term and Long Term Absence
Short term absence accounts for 71.38% of all cases, and some locations show concentrated long term absence among a small number of employees. Recommended actions:
•	Introduce clearer guidance for managing persistent short term absence.
•	Strengthen return to work processes and long term absence reviews.
•	Ensure consistency in how managers apply absence policies across departments.
4. Targeted Review of High Absence Areas (Derby and Engineering)
Derby has the highest absence rate (2.8%), and Engineering shows the highest departmental absence (3.02%). Absence in these areas is heavily driven by cold/flu and mental health, with a small number of employees contributing disproportionately.
Recommended actions:
•	Conduct focused reviews with managers in Derby and Engineering to understand local factors.
•	Assess whether support, workload, or working conditions differ from lower absence areas.
•	Provide targeted wellbeing interventions or manager coaching where needed.
5. Manage Seasonal Peaks in Absence
Absence spikes in May 2026 (1,287 days), March 2026 (1,259 days), and November 2025 (1,241 days). Recommended actions:
•	Plan additional staffing flexibility (e.g., overtime, temporary cover) during these months.
•	Consider offering additional leave or wellbeing days during peak periods.
•	Increase communication around wellbeing and illness prevention ahead of seasonal peaks.
6. Support Locations with High Long Term Absence (Leicester)
Leicester shows the highest long term absence levels, with the top 7 employees contributing 28.8% of total absence. Recommended actions:
•	Review long term absence cases individually to ensure appropriate support and interventions.
•	Strengthen occupational health referrals and early intervention pathways.
•	Provide managers with guidance on managing long term absence effectively.
7. No Action Required for Employment Type
There is no meaningful trend across employment types. Recommended actions:
•	Continue monitoring, but no immediate changes are required.
Next Steps: If we had more time/data, what else would we look into based on our current findings? 
If additional time or data were available, several enhancements could be made to deepen the analysis and expand the dashboard’s capability. These developments would allow leaders to explore absence patterns more comprehensively and support more targeted decision‑making.
1. Monthly and Seasonal Analysis
Introduce more detailed month‑by‑month breakdowns to help managers anticipate upcoming peaks based on historical patterns. This would support resource planning, overtime allocation, and wellbeing initiatives during high‑risk periods.
2. Year‑on‑Year Comparison
Once future years of data become available, add year‑on‑year visuals to identify whether absence trends are improving, worsening, or remaining stable. This would enable long‑term performance tracking and evaluation of interventions.
3. Enhanced Employee‑Level Insights
Expand employee‑specific analysis by incorporating additional demographic fields such as age groups, tenure groups, or contract types. This would allow more granular segmentation and help identify whether certain workforce groups are more prone to absence.
4. Policy Monitoring and Compliance
If new absence policies are introduced, integrate policy markers into the dashboard. This could include indicators showing when employees have triggered policy thresholds and whether appropriate actions (e.g., return‑to‑work meetings, occupational health referrals) have been completed.
5. Manager and Team‑Level Context
With more employee data, tooltips or drill‑through pages could include manager information, team structure, or department hierarchy. This would make it easier to identify where additional managerial support or intervention may be required.
6. Predictive or Forecasting Models
With sufficient historical data, simple forecasting models could be added to predict future absence levels. This would support proactive planning and help identify emerging risks before they materialise.
7. Root‑Cause Exploration
If more detailed absence reasons or HR case notes become available, deeper diagnostic analysis could be performed to understand underlying causes, patterns of recurrence, or links to workload, seasonality, or organisational change.

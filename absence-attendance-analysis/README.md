# Employee Absence & Attendance Analysis
# Business Problem
Northstar Distribution Group is a large UK organisation employing approximately 2,500 people across multiple departments, locations and shift patterns.
Senior leadership has noticed that employee absence has become a growing operational concern. Absence levels appear to vary considerably between departments, locations and working patterns, but the organisation currently lacks a clear, centralised view of the data.

# Project Objective
To analyse 12 months of employee absence data across Northstar Distribution Group, identify key trends and patterns, and provide leadership with actionable insights into the scale, causes and distribution of absence.
The project will involve data-quality assessment and cleansing using SQL, analytical modelling and visualisation using Power BI, and the development of evidence-based recommendations.

# Tools
- SQL: Data Cleansing
- PowerBI: Data Transformations and Visualisation

# Data Visualisation Approach
Key Visualisation Choices;
•	KPI Cards Used to present core metrics such as total absence days, number of absence records, average duration, absence rate, and number of employees with absence. KPI cards provide immediate, high level insight and act as entry points for deeper analysis.
•	Line Chart (Absence Trend Over Time) A 12 month line chart was used to show how absence days fluctuate month to month. This helps identify seasonal patterns, spikes, or improvements over time.
•	Donut Chart (Absence Category Breakdown) The split between short term and long term absence is shown using a donut chart, offering a clear proportional view of absence types.
•	Pie Chart (Top 5 Absence Reasons) A pie chart highlights the most common reasons for absence, enabling leaders to quickly identify key drivers such as cold/flu, mental health, or musculoskeletal issues.
•	Interactive Slicers Slicers for department, location, and employment type allow users to filter the dashboard dynamically. This supports targeted analysis and ensures the visuals respond to organisational structure.
•	Top 5 Employee Table A ranked table displays employees with the highest absence levels, supported by DAX measures that calculate contribution percentages and dynamic ranking. This helps identify individuals with significant absence impact.
•	Tooltip Pages Custom tooltip pages were added to provide additional context when hovering over employees, including profile information and absence contribution metrics. This enhances interactivity without cluttering the main dashboard.

Outcome:
The final dashboard provides a clear, interactive, and insightful view of absence trends across the organisation. Leaders can quickly identify patterns, compare departments or locations, and understand the drivers behind absence levels.
<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/34d8bb9d-9608-4656-8bf8-6860adc68758" />
<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/41c252bf-2435-458f-8a9a-8bc12f28b142" />

# Recommendations
- Provide additional support for cold and flu cases including provision of flu jabs
- Providing support for mental health amongst company benefits and promote the employee assistance programme.
- Implement short term and long term health policies
- Conduct reviews in Derby and Engineering as the highest absence location/department respectively, to understand if enough support is being provided by management.
- Manage seasonal peaks in absence via staffing (March, May, November)

# Next Steps
If I had more time and relevant data I would add the following; 
1. Monthly and Seasonal Analysis
Introduce more detailed month‑by‑month breakdowns to help managers anticipate upcoming peaks based on historical patterns. This would support resource planning, overtime allocation, and wellbeing initiatives during high‑risk periods.

3. Year‑on‑Year Comparison
Once future years of data become available, add year‑on‑year visuals to identify whether absence trends are improving, worsening, or remaining stable. This would enable long‑term performance tracking and evaluation of interventions.

5. Enhanced Employee‑Level Insights
Expand employee‑specific analysis by incorporating additional demographic fields such as age groups, tenure groups, or contract types. This would allow more granular segmentation and help identify whether certain workforce groups are more prone to absence.

7. Policy Monitoring and Compliance
If new absence policies are introduced, integrate policy markers into the dashboard. This could include indicators showing when employees have triggered policy thresholds and whether appropriate actions (e.g., return‑to‑work meetings, occupational health referrals) have been completed.

9. Manager and Team‑Level Context
With more employee data, tooltips or drill‑through pages could include manager information, team structure, or department hierarchy. This would make it easier to identify where additional managerial support or intervention may be required.

11. Predictive or Forecasting Models
With sufficient historical data, simple forecasting models could be added to predict future absence levels. This would support proactive planning and help identify emerging risks before they materialise.

13. Root‑Cause Exploration
If more detailed absence reasons or HR case notes become available, deeper diagnostic analysis could be performed to understand underlying causes, patterns of recurrence, or links to workload, seasonality, or organisational change.


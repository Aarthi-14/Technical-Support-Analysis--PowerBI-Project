# Technical Support Analysis - PowerBI Project

## Table of Contents
- [Project Overview](#project-overview) 
- [Data Sources](#data-sources)
- [Tools Used](#tools-used) 
- [Objective](#objective)
- [Methodology](#methodology)
  - [Data Cleaning and Transformation](#data-cleaning-and-transformation)
  - [Data Modelling](#data-modelling)
      - [Columns](#columns)
      - [Key Measures](#key-measures)
      - [DAX Calculated Columns](#dax-calculated-columns)
  - [Data Visualization](#data-visualization)
      - [OverAll KPI Dashboard](#overall-kpi-dashboard)
      - [Companys Perspective Dashboard](#companys-perspective-dashboard)
      - [Workforce Analysis Dashboard](#workforce-analysis-dashboard)
      - [Employee Wellness Dashboard](#employee-wellness-dashboard)
  - [Key Insights](#key-insights)
  - [Succession Planning](#succession-planning)
  - [Recommendations](#recommendations)
- [Conclusion](#conclusion)
  
### Project Overview
This data analysis project aims to provide valuable insights by analyzing the functioning of technical support to improve better Performamce & Customer Satisfaction.  

#### Data Sources
Employee data: The primary dataset is in CSV file "Onyx Data -DataDNA Dataset Challenge - Technical Support Dataset - May 2024.xlsx" containing Technical Support Details of the Organisation.

### Tools Used
- PowerBI Desktop : For Analysis

### Objective
This Analysis aims to analyzing the functioning of technical support through KPIs like Ticket Trends, SLA adherence patterns & CSAT score, etc.
This Interactive Dashboard answers the All the following Challenge Questions:
1. What are the peak ticket creation times?
2. How does the first response and resolution times compare against SLAs?
3. Explore customer satisfaction rates across agents, topics and other categories.

### Methodology

### Data Cleaning and Transformation
In the initial Data Preparation, we performed the following Tasks:
1. Data Loading and Inspection.
2. Handling Missing Values & Removing Duplicates Values and Removing Redundant Column
3. Data Transformation step like Changed Datatypes for necessary columns.

### Data Modelling
Since There is only one table containing all information, data modelling process included creating Key Measures and Calculated columns for Analysis.

Table: Technical Support Dataset
#### Columns
- Status,
- Ticket ID,
- Priority,
- Source,
- Topic,
- Agent Group,
- Agent Name,
- Created time,
- Expected SLA to resolve,
- SLA For Resolution,
- Resolution time,
- Expected SLA to first response,
- First response time,
- SLA For first response,
- Close time,
- Agent interactions,
- Survey results,
- Product group,
- Support Level,
- Country,
- Latitude,
- Longitude


#### Key Measures
1. Daily Ticket Volume:
   - Measures the Daily Volume of Tickets
   - Formula: Daily Ticket Volume = COUNTROWS('Data')
2. Monthly Ticket Volume:
   - Measures the Monthly Ticket Volume
   - Formula: Monthly Ticket Volume = CALCULATE([Daily Ticket Volume], VALUES('Calender'[Start of Month])) 
3. Weekly Ticket Volume:
   - Measures the Weekly Ticket Volume
   - Formula: Weekly Ticket Volume = CALCULATE([Daily Ticket Volume], VALUES('Calender'[Week]))
4. Resolved Tickets:
   - Gives the Resolved Tickets which adheres "Within SLA" of both SLA for First Response & SLA for Resolution.
   - Formula: Resolved Tickets = CALCULATE
                                 (COUNT(Data[Ticket ID]),
                                 Data[SLA For Resolution] = "Within SLA", 
                                 Data[SLA For first response] = "Within SLA", 
                                 Data[Status] = "Closed"|| Data[Status] = "Resolved")
     
5. In Progress Tickets:
   - Calculates the In Progress Tickets
   - Formula: In Progress Tickets = CALCULATE(COUNT(Data[Ticket ID]), Data[Status] = "In progress")
6. Peak Ticket Creation Hour:
   - Provides the Maximum Number of Tickets created during Peak Hours.
   - Formula: Peak_Ticket_Creation_Hour = 
               VAR HourlyTicketCount = SUMMARIZE('Data','Data'[Createdtime_Hour],"Ticket ID", COUNTROWS('Data'))
               VAR MaxTickets = MAXX(HourlyTicketCount,[Daily Ticket Volume])
               RETURN
               CALCULATE(MAX('Data'[Createdtime_Hour]),FILTER(HourlyTicketCount,[Daily Ticket Volume] = MaxTickets))
7. Work Hours Ticket Volume:
   - Indicates the ticket volume created during Work Hours.
   - Formula: Work Hours Ticket Volume = CALCULATE([Daily Ticket Volume], FILTER('Data',
                                                                          HOUR('Data'[Created time]) >= 9 &&
                                                                          HOUR('Data'[Created time]) < 17))
8. After Hours Tickets:
   - Provides the ticket volume created during  After Work Hours
   - Formula: After Hours Tickets = CALCULATE([Daily Ticket Volume],FILTER('Data',
                                                                    HOUR('Data'[Created time]) < 9 ||
                                                                    HOUR('Data'[Created time]) >= 17))
9.  Average First Response Time:
    - Measures the Average First Response Time.
    - Formula: Average First Response Time (Hours) = 
               VAR TotalFirstResponseTimeInSeconds = SUMX(
                                                          FILTER('Data', 
                                                          NOT(ISBLANK('Data'[Created date & time])) && 
                                                          NOT(ISBLANK('Data'[First response time])) ),
                                                          DATEDIFF('Data'[Created date & time], 'Data'[First response time], SECOND))
                                                          VAR CountFirstResponseTickets = COUNTROWS(
                                                          FILTER('Data', 
                                                          NOT(ISBLANK('Data'[Created date & time])) && 
                                                          NOT(ISBLANK('Data'[First response time])) ))
                                                          RETURN IF(CountFirstResponseTickets = 0, BLANK(), TotalFirstResponseTimeInSeconds / CountFirstResponseTickets / 3600)
10.  Average Resolution Time:
    - Measures the Average Resolution Time.
    - Formula: Average Resolution Time (Hours) = 
                                                VAR TotalResolutionTime = SUMX(FILTER('Data', NOT(ISBLANK('Data'[Resolution time])) && NOT(ISBLANK('Data'[Created date & time]))),
                                                                                        DATEDIFF('Data'[Created date & time], 'Data'[Resolution time], SECOND))
                                                VAR CountResolvedTickets = COUNTROWS(FILTER('Data', NOT(ISBLANK('Data'[Resolution time])) && NOT(ISBLANK('Data'[Created date & time]))))
                                                RETURN IF(CountResolvedTickets = 0, BLANK(), TotalResolutionTime / CountResolvedTickets / 3600)

11. Weekly vs Weekend Ticket Volume:
    - Indicates the ticket Volume of Weekdays or Weekend. 
    - Formula:Weekday vs. Weekend Ticket Volume = AVERAGEX('Calender', IF('Calender'[Weekday or Weekend] = 1, [Daily Ticket Volume], BLANK()))
    - 
12. CSAT Score (%) Index:
    - Represents the Customer Satisfaction towards the Agent Response.
    - Formula: CSAT Score (%) = VAR TotalResponses = COUNTAX('Data', 'Data'[Survey results])
                                VAR SatisfiedResponses = CALCULATE(COUNTAX('Data', 'Data'[Survey results]),'Data'[Survey results] >= 4)
                                RETURN DIVIDE(SatisfiedResponses, TotalResponses, 0)

13. SLA Violated for First Response:
    - Represents the SLA Violation for First Response.
    - Formula: SLA Violated for First Response = CALCULATE(COUNT(Data[Ticket ID]), Data[SLA For first response] = "SLA Violated")
14. SLA Violated for Resolution:
    - Represents the SLA Violation for Resolution.
    - Formula: SLA Violated for Resolution = CALCULATE(COUNT(Data[Ticket ID]), Data[SLA For Resolution] = "SLA Violated")
15. Within_SLA First Response:
    - Measures the Within_SLA First Response
    - Formula: Within_SLA First Response = CALCULATE(COUNT(Data[Ticket ID]), Data[SLA For first response] = "Within SLA")
16.  Within_SLA for Resolution:
    - Indicates the Within_SLA for Resolution.
    - Formula: Within_SLA for Resolution = CALCULATE(COUNT(Data[Ticket ID]), Data[SLA For Resolution] = "Within SLA")


### Data Visualization
Introduction:
Visualizations play a crucial role in translating raw data into actionable insights. In this section, we will explore the key visualizations used to analyze Technical Support trends.
The Dashboard consists of 3 sections.

Section 1 - Ticket Summary  
      ![Section1](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/86144f09-adae-4e28-af8d-1fd6e9a11355)

Section 2 - Ticket Trends

      ![Section2](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/9c99cda2-36c8-496b-a9c6-d0064d06ae24)

Section 3 - CSAT & Agent SLA Trends

      ![Section3](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/10175e32-fbbd-43c1-a3c0-567578cab53f)



### Ticket Summary
This visualization presents a comprehensive view of Overall Ticket Summary. 
- Key metrics include Total Tickets, Resolved Tickets, In Progress Tickets, Peak Ticket Creation Time,Work Hour Tickets, After Hour Tickets, Average FRT, Average RT.
- Users can Interact with Visualization by using slicers for Month.
  
      ![Ticket summary](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/0f4cf38c-7fc4-42a9-8191-1ab519f5a35a)

      
 2.  Attrition count by Department:
    - Used Funnel Chart for this visualization that gives attrition trends by department.
  
       ![Attritionbydept](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/326d6a9f-9e4b-4351-b25c-7369a8d4f4a5)
 
 4. Attrition by Job Role:
    - Used Clustered column Chart for this visualization that gives attrition count of the Employees by Job Role.
  
      ![attritionbyjobrole](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/4c9bca1b-83ee-4286-b2f0-b21c8acac31f)
 
 7. Attrition by Gender:
    - Used Donut Chart for this visualization that gives attrition count of the Employees by Gender.

      ![attrition by gender](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/48a008b6-30fb-4f40-96ac-19978aa735ef)

8. Attrition by Age Category:
    - Created Calculated DAX column for Age category.
    - Used Donut Chart for this visualization that gives attrition count of the Employees by Age Category.
  
      ![attrition by age](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/b7ef8e66-85c6-40f8-8fc3-51db87867059)

 9. Attrition by Education:
    - Used Clustered Bar Chart for this visualization that gives attrition Rate of the Employees by Education.

      ![Attrition by education](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/bc60651e-8af8-4b28-828c-e3326a8c5fd3)


### Companys Perspective Dashboard

This dashboard presents a comprehensive view of overall attrition trends. 
- By establishing a relation, with Attrition Rates to various metrics such as Average Monthly Salary, Average Monthly Rate, Average Training times last year and Average Work-Life Balance gives key valuable Insights that leads to higher Attrition Rates.
- Users can Interact with any Visualization by using slicers for Gender/Department/Job Role.Implemented Slicers for filtering insights by Attrition, Department, Gender, and Job Role.

![company's perspective](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/a79c3674-9043-4cd0-8c3f-91bf2777da3a)

 1. Attrition Rate Vs Average Monthly Salary by Job Role:
    - In this Line Chart Visual, Average Monthly of the Employees by Job Role is compared with Attrition Rate which provides an information that the Attrition Rate is higher for the Job Roles with Lower Pay such as Sales Representative and Lab Techncian role.
    - Users can Interact with any Visualization by using slicers for Gender/Department.

    ![2023-12-14_21h31_17](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/3c4d31a2-583d-4426-b81a-029204071173)

 2. Attrition Rate Vs Average Monthly Salary and Average Rate by Job Role:
    - In this Line and Clustered Column Chart Visual, Average Monthly Salary of the Employees and Average Rate by Job Role is compared with Attrition Rate which provides an information that the AVerage Monthly Salary is too low comparing to Average Monthly Rate fixed by the Company for the Job Roles Sales Representative and Lab Technician. It also corresponds to Higher Attrition Rate.
    - Users can Interact with any Visualization by using slicers for Gender/Department.

    ![Avg Rate](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/006b5b0a-f222-47ba-8b17-77aff4c3f66a)

 3. Attrition Rate Vs Average Training times Last year by Job Role:
    - In this Line Chart Visual, Average Training Times of the Employees by Job Role is compared with Attrition Rate which provides an information that the Job Role Sales Representative has Undergone Highest Training Times last Year has Highest Attrition Rate which clearly shows that Company incured Loss on Training Cost for these Job Roles.
    - Users can Interact with any Visualization by using slicers for Gender/Department.

    ![AVG training](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/e4623bee-c3ab-408e-9715-57c88c4c6a43)

   
 4. Attrition Count Vs Average Work Life Balance:
    - Used Donut Chart for this visualization that gives attrition count based on the Average Work Life Balance.
    - Users can Interact with any Visualization by using slicers for Gender/Department.

    ![Work life balance](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/4857db27-9f44-4682-a63a-36416d1945d0)

### Workforce Analysis Dashboard

This dashboard presents a comprehensive view of overall Workforce trends by Attrition. 
  - Key metrics include Average Age, Average Percentage Hike, Average Monthly salary, Environment satisfaction Index, Job satisfaction index, Job Involvement Index, Average Tenure at the Company by Job Role are there in this Dashboard.
  - Users can Interact with any Visualization by using slicers for Gender/Department/Job Role/Attrition.

    ![Workforce Analysis](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/cc5b2522-bff1-46bc-97b6-1c3b9cdd3cb7)

1. Average Percentage Hike, Average Monthly Salary, Average Age, Average Distance From, Job Satisfaction Index, Job Involvement Index, Average Tenure at Current Role.
   - Used Card Visuals for these metrics.
   - User can interact with these Metrics by using Slicers For Department/Gender/Attrition/Job Role.

     ![avg PERCT-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/d147d480-368f-44f7-9a1f-606e1d083c7f)
     ![Avg monthly sal-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/07d81b7f-4129-4344-a34d-ab402c03eb46)
     ![AVG_tENURE-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/fd985374-2669-4053-847a-325f9d7f5ea5)
     
     ![Avgdistance-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/4c2ed189-a0e8-49cb-9b7a-4a846e14f3ca)
     ![Job involve-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/09905168-4d74-40f0-ba9b-082c680109b5)
     ![job satis -3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/3a0f875d-3920-4f43-bc99-78cdc9afcffb)

2. Average Age.
   - Used Gauge Visual for this Metric.
   - User can interact with these Metrics by using Slicers For Department/Gender/Attrition/Job Role.

     ![AVG age-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/bced4f37-7c25-454c-a88b-960bcccc175a)

3. Average Tenure at the Company by Job Role:
   - Used clustered Bar Chart for this visual to analyze Employees Tenure Patterns Based on the Job Role.
   - User can interact with these Metrics by using Slicers For Department/Gender/Attrition/Job Role.
  
     ![Avg tenure by job-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/71d7476b-6862-45be-b1ff-8edfce67bae8)

4. Average Tenure since Last Promotion by Job Role Vs Attrition Rate:
   - Used Line and clustered Column Chart for this visual to analyze Employees Tenure Patterns Based on the Job Role with Attrition Rate.
   - User can interact with these Metrics by using Slicers For Department/Gender/Attrition/Job Role.

     ![avg tenurelastpromo-3](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/296881ca-ddf9-400f-96cd-7f512b192465)


### Employee Wellness Dashboard
This dashboard presents a comprehensive view of overall Employee Wellness.
     - Analysing by the Key metrics include Relationship Satisfaction Index, Performance Satisfaction Index,Environment Satisfaction Index, Overtime Rate, Training Rate, Business Travel, Managerial Change Rate to find the Impact On Higher Attrition Rates.
     - Users can Interact with Visualization by using slicers for Gender/Department/Job Role.


 1. Overtime Rate, Training Rate, Average Tenure since Last Promotion, Relationship Satisfaction Index, Environment Satisfaction Index, Performance Rating Index.
    - Used Gauge Visuals for these metrics.
    - User can interact with these Metrics by using Slicers For Department/Gender/Attrition/Job Role.

      ![overtimerate-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/4847c05a-073c-4c4c-8761-631e61da916c)
      ![trainingrate-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/145f6c29-daed-4f9f-9bdd-b703c46f699b)
      ![avgtenure-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/784afa31-70e0-45cf-9cb6-328aa8278c88)
      
      ![Relationshipindex-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/2944718d-d0f3-44db-903f-92cab1d798e5)
      ![EnvironmentSatisfaction-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/52216090-2083-41d3-849c-773c6aed1871)
      ![performancerating-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/2311c7a7-abff-4407-a262-f97c7c2a3d5e)

 2. Attrition Rate Vs Business Travel by Job Role:
    - In this Line and Stacked Column Chart Visual, Business Travel of the Employees by Job Role is compared with Attrition Rate which provides an information that the Job Role Sales with Highest Business Travel has Highest Attrition Rate.
    - Users can Interact with any Visualization by using slicers for Gender/Department.
      
     ![Businesstravel-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/9a0013ad-d0a8-4462-acfb-c8ef93abb7a3)

3. Managerial Change Rate Vs Attrition Rate:
    - In this Line and Clustered Column Chart Visual, Mnagerial Change Rate of the Employees by Job Role is compared with Attrition Rate which provides an information that the Job Role Sales with Highest Business Travel has Highest Attrition Rate for the Sales Representative Job Role.
    - Users can Interact with any Visualization by using slicers for Gender/Department.

     ![Managerialchange-4](https://github.com/Aarthi-14/HR_Attrition_Analysis-PowerBI/assets/147639053/4e7177d9-e485-44f7-a4c1-3be82d88c686)

### Key Insights

#### Attrition Rate Trends:
- Out of 1470 Total Employees, 237 Employees left the company which estimates 16.12 % Attrition Rate.
#### Departmental Variances:
- 133  is the Attrition count in R&D Department, followed by Sales Department with 92 Attrition count.
- Lab Technician is the Job Role which has the Highest Attrition Count of 62 in R&D Department with Average Monthly salary of 3237.14.
- Sales Representative is the Job Role which has Highest Attrition Rate of 39.76% with Average Monthly salary of 2626.
#### Demographic Analysis:
- Attrition by Gender gives an Insight that out of 237 Employees, 150 are the Male Employees and 87 are the Female Employees.
- The Average Age of the Employees Under Attrition Category is 34.
#### Correlation with Satisfaction Surveys:
- The Job Satisfaction Index is 2 (Employees under Attrition Category) which shows Employees are dissatisfied with their Job.
- The Average Work-Life Balance of the Employees under Attrition category is 3(Average).
#### Identification of High-Risk Roles:
- Sales Representative, Sales Executive, Lab Technician Job Roles are at Higher likelihood of Attrition by Job role.
#### Cost Analysis:
 -  As per the Analysis, Sales Representative and Lab Technician are the Job roles that undergone Highest Average Training Times Last Year have the Highest Attrition Rate which indicates that company has incured loss on both Recruitment cost and Training cost.

### Succession Planning Opportunities:

1. Identifying High-Potential Employees
2. Developing Leadership Training Programs
3. Creating Talent Pipelines
4. Cross-Training and Skill Development
5. Knowledge Transfer Programs
6. Succession Planning for Critical Departments
7. Creating Mentorship Programs
8. Encouraging Career Development Discussions
9. Internal Promotions and Advancements
10. Strategic Recruitment for Critical Roles
11. Employee Engagement Initiatives

### Recommendations

 #### Competitive Compensation Strategies:
Keep up with market rates to ensure employees receive competitive salaries and comprehensive compensation packages.
 #### Career Growth Opportunities:
Foster an environment that provides clear pathways for career advancement and growth within the organization.
 #### Cultivating a Positive Workplace Culture:
Actively work to improve workplace culture, creating an environment that promotes collaboration, inclusion, and employee satisfaction.
 #### Prioritizing Work-Life Balance:
Recognize and prioritize the importance of work-life balance to enhance employee well-being and job satisfaction.
 #### Recognition and Rewards Programs:
Implement effective programs to recognize and reward employees for their contributions and achievements.
 #### Strategic Recruitment Practices:
Ensure that the recruitment process aligns with the specific job roles, bringing in individuals who are well-suited for the positions.
 #### Leadership and Management Transformation:
Overhaul leadership and management styles to foster a more efficient, supportive, and empowering work environment.
 #### Flexible Work Arrangements for Overtime:
Offer flexibility to employees who work overtime, allowing for adaptable work arrangements that accommodate their needs

### Challenges faced
Navigating data challenges, including quality issues and limited historical data, integrating diverse data sources, Communicating complex findings effectively and adapting to unexpected external factors. Balancing technology/tool challenges, obtaining comprehensive employee feedback, and aligning analysis with organizational goals pose additional complexities.

### Conclusion:
In conclusion, this HR Attrition Analysis project has provided valuable insights into the organization's workforce dynamics. By analyzing trends, identifying key factors contributing to attrition, and offering actionable recommendations, this analysis contributes to informed decision-making. We acknowledge the lessons learned during this project and look forward to applying these insights in future HR analytics initiatives.


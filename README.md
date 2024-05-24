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
  - [Data Visualization](#data-visualization)
      - [Ticket Summary](#ticket-summary)
      - [Ticket Trends](#ticket-trends)
      - [CSAT & Agent SLA](#csat-&-agent-sla)
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
      FILTER('Data', NOT(ISBLANK('Data'[Created date & time])) && 
      NOT(ISBLANK('Data'[First response time])) ),
      DATEDIFF('Data'[Created date & time], 'Data'[First response time], SECOND))
      VAR CountFirstResponseTickets = COUNTROWS(
      FILTER('Data', NOT(ISBLANK('Data'[Created date & time])) && 
      NOT(ISBLANK('Data'[First response time])) ))
      RETURN IF(CountFirstResponseTickets = 0,BLANK(),TotalFirstResponseTimeInSeconds/CountFirstResponseTickets / 3600)  
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

### Section 1 - Ticket Summary
This section gives a summary of the Ticket volumes, the daily, monthly & weekly ticket Volume & the Peak Ticket Creation hour trend.

![Section1](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/86144f09-adae-4e28-af8d-1fd6e9a11355)

### Section 2 - Ticket Trends
This section gives a detailed view of the Ticket trends over Topic, Source, Product & Geography & SLA Pattern for First Response & Resolution.

![Section2](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/9c99cda2-36c8-496b-a9c6-d0064d06ae24)

### Section 3 - CSAT & Agent SLA Trends
This section gives a detailed view of the CSAT Trend for Topic, Agent & Agent SLA for First Response & Resolution.

![Section3](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/10175e32-fbbd-43c1-a3c0-567578cab53f)



### Ticket Summary
This visualization presents a comprehensive view of Overall Ticket Summary. 
- Key metrics include Total Tickets, Resolved Tickets, In Progress Tickets, Peak Ticket Creation Time,Work Hour Tickets, After Hour Tickets, Average FRT, Average RT.
- Users can Interact with Visualization by using slicers for Month.
  
      ![Ticket summary](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/0f4cf38c-7fc4-42a9-8191-1ab519f5a35a)


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


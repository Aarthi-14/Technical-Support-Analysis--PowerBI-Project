# Technical Support Analysis - PowerBI Project
 
## Created by Aarthi Duraisingam [Linkedin Profile](https://www.linkedin.com/in/aarthi-duraisingam-2438b7248/)
  Technical support, also known as tech support, is a call centre type customer service provided by companies to advise and assist registered users with issues concerning their technical products. This Analysis aims to analyzing the functioning of technical support through KPIs like Ticket Trends, SLA adherence patterns & CSAT score, etc.

  ![Onyx Technical Support Analysis_May'24 - Aarthi](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/a73fa774-1bd8-4d81-a485-813f7d66793e)

### Table of Contents 
- [Project Overview](#project-overview) 
- [Data Sources](#data-sources)
- [Objective](#objective)
- [Comprehensive-Data-Analysis](comprehensive-data-analysis)
	- [Ticket Summary](#ticket-summary)
	- [Ticket Trends](#ticket-trends)
	- [CSAT & Agent SLA](#csat-&-agent-sla)
- [Key Insights](#key-insights)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)
  
### Project Overview
This data analysis project aims to provide valuable insights by analyzing the functioning of technical support to improve better Performamce & Customer Satisfaction.  

#### Data Sources
Technical Support data: The primary dataset is in CSV file "Onyx Data -DataDNA Dataset Challenge - Technical Support Dataset - May 2024.xlsx" containing Technical Support Details of the Organisation.

### Objective
This Analysis aims to analyze the functioning of technical support through KPIs like Ticket Trends, SLA adherence patterns & CSAT score, etc.
This Interactive Dashboard answers the All the following Challenge Questions:
1. What are the peak ticket creation times?
2. How does the first response and resolution times compare against SLAs?
3. Explore customer satisfaction rates across agents, topics and other categories.

### Comprehensive Data Analysis:
Visualizations play a crucial role in translating raw data into actionable insights. In this section, we will explore the key visualizations used to analyze Technical Support trends.The Dashboard consists of 3 sections.

### Ticket Summary
This section gives a summary of the Ticket volumes, the daily, monthly & weekly ticket Volume & the Peak Ticket Creation hour trend.

  ![TC SUMMARY](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/66d21c3d-64f7-489c-b81e-b940bccc6272)


### Ticket KPIs
This visualization presents a comprehensive view of Overall Ticket Summary. The Key metrics include Total Tickets, Resolved Tickets, In Progress Tickets, Peak Ticket Creation Time,Work Hour Tickets, After Hour Tickets, Average FRT, Average RT.
  
  ![Ticket1](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/49d36686-d5b7-45b4-b5fe-3cdd5ced0d52)

  * Out of ﻿2330﻿ total tickets,﻿1355﻿ tickets are Resolved based on " Within SLA" of both SLA for First Response & SLA for Resolution. 
  * In Progress tickets are in total of ﻿400﻿.
  * The Overall Peak Ticket Creation Hour is at ﻿15﻿ Hours in which Maximum number of tickets has been raised during that Hour.
  * Maximum Tickets were raised during After Hours of ﻿1566﻿ in total & ﻿764﻿ Tickets in total was raised during Work Hours.
  * The  Average First Resolution Time(Hours) is ﻿0.43﻿ & the Average Resolution Time (Hours) is ﻿33.24﻿.
    
#### Daily Ticket Volume

![Daily tc](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/73603ae9-ff2b-4e4c-9869-dbb50178d5ad)

  * Daily volume trends shows maximum number of Tickets(439) was created on Wednesdays. During Saturdays & Sundays, the tickets were less than 200 in total.

#### Weekly Ticket Volume
  ![weekly tc](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/4c439b98-8c31-4562-9b8e-929d8e16c073)

  * Weekly volume trends shows maximum number of Tickets(63) was created on Week Number 4.

#### Monthly Ticket Volume 
  ![Monthly tc](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/946ceac7-1c24-4f6a-9436-f66b907dc666)

  * Monthly Ticket Volume peaks at January 2023 with 224 tickets.

#### Peak Ticket Creation Hour:
  ![Peak tc](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/2304e067-47a4-44b5-9bff-e5ad7b4fd0a0)

  * The Overall Peak Ticket Creation Hour is at ﻿15﻿ Hours in which Maximum number of tickets(135 tickets) has been raised during that Hour.
    
### Ticket Trends
This section gives a detailed view of the Ticket trends over Topic, Source, Product & Geography & SLA Pattern for First Response & Resolution.

  ![tc trend](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/105b1755-fa33-4dec-9bbe-76fa80beffee)

#### Topic Trend
  ![Ticket Trends](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/46af544f-63a8-4b35-8fb6-5de4df2bf569)

  * Under Product Setup Topic, out of 630 (Maximum) tickets, 374 tickets  are Resolved based on " Within SLA" of both SLA for First Response & SLA for Resolution.
  * Out of 525 tickets, 300 tickets are resolved in Pricing & licensing category.

#### SLA for First Response & SLA for Resoltion
  ![sla](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/274f679a-9e50-462d-928a-e1aa461c9c20)

*  ﻿SLA for First Response Category has 2019 tickets are under "Within SLA"  & 311﻿ Tickets are SLA Violated.
*  ﻿SLA for Resolution Category has 1547 tickets under "Within SLA" & 783﻿ Tickets are SLA Violated.

#### Source & Product Trend
  ![source   Product](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/6140ce4c-b4c1-4473-9378-254b3e357a26)

  * Out of ﻿1234﻿ Email Tickets, ﻿780﻿ are Resolved and ﻿844﻿ are tickets raised during After Hours.
  * Ready to use Software is the Product Category in which Maximum Number of ﻿1010﻿ Tickets were raised, in which ﻿610﻿ tickets were Resolved both based on " Within SLA" of both SLA for First Response & SLA for Resolution. 

#### Geography Trend of SLA for First Response
  ![Geography Trends](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/885b9c78-24c8-437d-9401-26255b70eb57)

* Germany has the Hightest ticket volume of 306 in which 259 tickets are "within SLA" & 47 tickets are SLA violated for First Response & 196 tickets are "within SLA" & 110 tickets are SLA violated for Resolution.
* Bulgaria has the lowest ticket ticket volume of 131 in 113 tickets are "Within SLA" & 18 tickets are SLA violated for first Response & 97 tickets are "within SLA" & 34 tickets are SLA violated for Resolution.
  
### CSAT & Agent SLA Trends
This section gives a detailed view of the CSAT Trend for Topic, Agent & Agent SLA for First Response & Resolution.

  ![Section3](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/10175e32-fbbd-43c1-a3c0-567578cab53f)

#### CSAT Trend
  ![csat score](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/65d8bb9f-6e9c-4c0e-b87c-bbabe79b7cf5)

 * The Overall CSAT Score marked 57% from 2nd January 2023 to 31st December 2023.
 * CSAT trend records the Highest of ﻿62 %﻿ in the Month of December.
 * The CSAT Score of Purchasing and Invoicing records to ﻿65 %﻿ which is the Highest in Topic Category.

  ![bEST AGENT](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/9468d044-160c-4b55-b3fa-2f91cee579fe)

 * Connor Danielovitch ranked Overall Highest CSAT Score of ﻿69.01%﻿ & he got 100% CSAT Score in Training Request & he tracked more than 60% of CSAT Score in each topic.

#### Agent SLA for First Response
  ![Agent SLA frt](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/4cdc1913-9bac-438c-bc7d-2a191d8b8b5f)
  
 * Michele Whyatt attended 90% of tickets within SLA for First Response and Minimum of 18 tickets, under "SLA violated for First Response" out of his Total 186 Tickets.

#### Agent SLA for Resolution
  ![Agent SLA RT](https://github.com/Aarthi-14/Technical-Support-Analysis/assets/147639053/f098dccc-8559-4068-9da9-5a14b6efc661)

 * Heather Urry attended maximum(76%) of tickets within SLA for Resolution and Minimum of 42 Tickets, under "SLA Violated for Resolution" out of his total 177 tickets.

### Key Insights
1. Resolution and SLA Adherence:
Out of 2330 total tickets, 1355 were resolved within the SLA for both first response and resolution times.
400 tickets are still in progress, highlighting a need for improved resolution processes.

2.Ticket Volume Trends:
The peak ticket creation hour is at the 15th hour, with the majority of tickets (1566) being raised during after-hours, compared to 764 during work hours.

3.Topic and Product Analysis:
Under the Product Setup topic, 374 out of 630 tickets were resolved within SLA, but significant SLA violations were noted: 311 for first response and 783 for resolution.
The Ready to Use Software category saw the highest number of tickets (1010), with 610 resolved within SLA.

4.Source Analysis:
Email was the predominant source of tickets (1234), with 780 resolved within SLA and 844 raised after hours.

5.Customer Satisfaction (CSAT) Analysis:
The overall CSAT score is 57%, with the highest monthly score of 62% recorded in December.
Connor Danielovitch achieved the highest individual CSAT score of 69.01%.
The Purchasing and Invoicing topic recorded the highest CSAT score of 65%.

6.Agent Performance:
Michele Whyatt had the fewest SLA violations for first response (18 out of 186 tickets).
Heather Urry had the fewest SLA violations for resolution (42 tickets).

### Recommendations
#### 1. Implement an improved after-hours support strategy.
Since the maximum number of tickets (1566) were raised during after-hours, consider increasing staffing or offering automated support solutions during these times to handle the high volume effectively.
#### 2. Optimize First Response Time (FRT):
Conduct training sessions focused on improving first response times. Introduce performance incentives for agents who consistently meet or exceed FRT SLAs. 
#### 3. Enhance Resolution Time:
Identify bottlenecks in the resolution process and implement process improvements. This could include better documentation, enhanced troubleshooting tools, and more efficient escalation procedures.
#### 4. Focus on High-Volume Topics and Categories:
Develop detailed knowledge base articles and self-service resources for these high-volume areas. Provide specialized training for support agents on these topics to enhance their expertise and efficiency.
#### 5. Monitor and Enhance CSAT Scores:
Regularly review customer feedback to identify areas for improvement. Implement follow-up procedures for negative feedback to resolve customer issues and improve satisfaction. Recognize and reward agents with high CSAT scores to encourage positive performance.
#### 6. Enhance Email Support Efficiency:
Streamline the email ticketing process with templates and automated responses for common queries. Ensure there is adequate after-hours email support to handle the volume effectively.
#### 7. Encourage Customer Feedback and Survey Participation:
The dataset includes many empty survey values, indicating that a significant number of customers did not participate in the survey. Actively encourage customers to provide their valuable feedback and participate in surveys regarding the agent service.

### Challenges faced
The dataset contains numerous empty survey responses, indicating a significant number of customers did not participate in the survey. This lack of participation poses a challenge in accurately analyzing agent performance, as the incomplete data may not fully reflect customer satisfaction levels and feedback.

### Conclusion:
In conclusion, this technical support analysis project has provided valuable insights into key performance metrics and trends within the support team. By leveraging comprehensive data analysis, we have identified areas of strength and opportunities for improvement, which can significantly enhance the efficiency and effectiveness of the technical support operations.By implementing these recommendations, the technical support team can enhance their performance, resulting in a potential increase in CSAT scores and overall customer satisfaction. This project has laid a solid foundation for ongoing analysis and improvement in technical support operations.

#### Created & Presented by -Aarthi Duraisingam @ Aspiring Data Analyst
Date- 22/05/2024

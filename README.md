# Project Description

Activate Company was established in 2012 and is known for providing financial services to individuals and businesses across the world. **In 2024, the company had 5,654 active employees in the UK, but 980 employees left the UK branches that year.** This shows that 17% of the workforce exited.

This sharp rise in employee exits has caused a serious problem for the company. Replacing these employees has been extremely expensive. In 2024 alone, the **UK branches spent £13.7 million on hiring and training new workers** to fill the gaps left by those who left.

This project was carried out to understand how the Activate Company is doing and why so many employees are leaving. 

<br/>

### The main objectives are to:

* Analyze the **financial impact** of employee turnover.

* Identify the departments with the **highest turnover**.

* Examine the **reasons why** employees are leaving these departments rapidly.

* Provide **recommendations** to reduce turnover.

<br/>

*Interact with the dashboard [here](https://app.powerbi.com/view?r=eyJrIjoiNmU2ZDhhMWUtNmY3MC00MzA5LTk0ZGQtNTBkMGJkYzIwZjdkIiwidCI6IjY5M2I4NzFiLTNhMjItNDUxOS04ZjZhLTFhYjNjOTI4Y2FlMSJ9)!*

*Download PDF Report [here](https://ooojege-my.sharepoint.com/:b:/g/personal/blessing_ooojege_onmicrosoft_com/EX1ZCs1y0rBEikvnUgISoEcB7GORZ9KEA5N9cEmQG2sq6g?e=YZriC2)!*

<br/><br/>

# Data Structure

The dataset contains four connected tables, each serving a unique role in explaining why employees are leaving and what factors are driving replacement costs.

<br/>

The **Fact table** captures the core experience and performance measures of employees. It includes **engagement scores, satisfaction with managers, onboarding quality, peer support, and performance ratings**. It also tracks whether **employees had a mentor, participated in mentorship programs, received promotions or recognition, as well as how long it took to get a first promotion and how many training hours were completed in the first month**. 

<br/>

The **Employment_Status table** records hire and exit details for every employee. It provides the **hire date, exit date, tenure, reasons for leaving, whether an exit interview was completed, the sentiment of those interviews, and whether there was a performance drop before leaving**. Importantly, it also **assigns replacement costs for each exit**.

<br/>

The **Employees table** adds demographic and role-based information. It contains **employee age groups, gender, education level, department, job role, location, and work arrangement (remote, hybrid, or onsite)**. It also records **salary details such as starting pay, market benchmarks, and years of prior experience**.

<br/>

The Date table provides the timeline dimension for the project. It organizes events by day, month, quarter, and year, while also allowing analysis along fiscal calendars. This table is key for identifying when attrition spikes occur, whether exits cluster in specific months or quarters, and how hiring compares to exits over time. By aligning attrition with time, the business can see if patterns are seasonal, linked to policy changes, or related to external market conditions.

<br/>

Together, these tables connect the **“who” (Employees), the “what” (Fact), and the “why” (Employment_Status)**. This structure makes it possible to analyze employee turnover as a story of experiences, costs, and workforce gaps that are affecting Activate Company’s UK branches.

![Data Structure](https://github.com/Blessing336/Activate-s-Employee-Turnover-Analysis/blob/df4ca5cf1772174dcb3525f5c0207f4931193081/Data%20Structure.png)

<br/><br/>

# Technical Details

Tools Used:

**Power BI (Data Model, DAX, Visualization) for creating new calculated columns, measures, relationships, and building dashboards.**

<br/>

Process Workflow:

* **Imported raw dataset into Power BI.**

* **Cleaned data (standardized categorical columns).**

* **Created additional columns (Age Sort, Education Sort, Tenure, Years in Company).**

* **Built custom Date table for time-based reporting.**

* **Modeled relationships between Fact, Employees, Employment_Status, and Date.**

* **Created DAX measures to quantify attrition, tenure, engagement, costs, promotions, and recognition.**

* **Designed 3 report pages in Power BI, syncing slicers across all pages for consistency.**

<br/>

Data Cleaning & Preparation:

**1. Data Import:**

* Loaded the Excel dataset into Power BI: Verified column data types (dates → date type, salaries → numeric, categorical fields → text).

<br/>

**2. Data Quality Checks:**

* Removed duplicate Employee IDs.

* Fixed spelling and inconsistency issues in categorical fields (e.g., Job Role, Department, Location).

* Filled missing values for Exit Interview Sentiment with “No Interview” category to avoid blank outputs in reports.

* New Columns (Created for Better Analysis):

<br/>

Employees Table:

**Age Sort** → numeric column created to correctly order Age Groups in visualizations (e.g., 20–29 before 30–39).

**Education Sort** → numeric column created to sort Education Levels (High School → Bachelor’s → Master’s → PhD).

<br/>

Employment_Status Table:

**Tenure** → calculated as difference between Hire Date and Exit Date (or Today (Day of Analysis) if still employed). This gives exact length of employment for each employee.

**Years in Company** → grouped the Tenure column into bands (0–1, 2–5, 6–10, 10+). This helps identify which employee groups are most at risk of leaving.

<br/>

Date Table:

→ Built a custom Date table (using M codes).

<br/>

![date](https://github.com/Blessing336/Activate-s-Employee-Turnover-Analysis/blob/4b41feda8f1b941c0ccd6d42bcf0087867e93f69/Date%20Table%20created.png)

<br/>

→ Added columns for Year, Month, Quarter and Week of Year.

→ Ensured the Date table covers the entire time range of employee hire and exit dates to give more granular control over time-series analysis (attrition per month, hires vs exits by quarter).

→ Synced Year slicers across all three report pages to ensure all visuals update consistently when a year filter is applied.

<br/>

![sync](https://github.com/Blessing336/Activate-s-Employee-Turnover-Analysis/blob/221409c822510404475aaf5fd214ace66d65b704/sync%20slicer%20image.png)

<br/>


**3. Data Modeling:**

* Set Employee ID as the key connecting Employees, Fact, and Employment_Status tables.

* Linked Hire Date and Exit Date from Employment_Status to the Date table for time-based analysis.

<br/>

**4. Some Measures Created (DAX):**

* Turnover Rate =

      VAR Employees_at_beginning = [Employees_at_beginning]
      
      VAR Employees_at_end = [Employees_at_end]
      
      VAR AvgEmp = DIVIDE(Employees_at_beginning + Employees_at_end, 2)
      
      VAR Exitemp = [Exited Employees]
      
      RETURN divide(Exitemp, AvgEmp)

* Replacement Cost = SUM of Replacement Cost column (from Employment_Status).

* Exit Sentiment Index = Percentages per departments.

* Sentiment Percent Color = 

      VAR Sentiment_Count_Exited = 
      CALCULATE(
          [Exited Employees],
          Employment_Status[Exit Year] <> BLANK()
      )
      
      VAR hmmn =
      CALCULATE([Exited Employees], Employment_Status[Exit Interview Sentiment] <> BLANK(), ALLEXCEPT('Date', 'Date'[Year]))
      
          RETURN 
      DIVIDE(Sentiment_Count_Exited, hmmn)

* Promotion Rate = % of employees with Promotion Received = Yes.

* Recognition Rate = % of employees with Recognition = Yes.

* Career Satisfaction Avg = Average of Satisfaction with Career Path.

* Created DAX measures to quantify attrition, tenure, engagement, costs, promotions, and recognitions.

* Designed 3 report pages in Power BI, syncing slicers across all pages for consistency.

<br/>

![measure](https://github.com/Blessing336/Activate-s-Employee-Turnover-Analysis/blob/4004c4f562160db63ff688a6570b8f4dc0938186/measure2.png)


<br/><br/>
# Executive Summary

Activate Company faced a severe retention crisis in 2024, spending **£13.7 million on replacing 980 employees**, a 493% increase from 2023. The **highest turnover was seen in Customer Support and Sales**, with Customer Support alone costing the company £4.4 million, accounting for 32% of the total replacement cost. Sales followed, contributing £2.95 million, emphasizing the urgent need for intervention in these departments.

The primary reasons for exit are **low salary, lack of promotion paths, poor onboarding, and weak mentorship participation**. Customer Support and Sales again topped the list, with 35% of exits in Customer Support and 26% in Sales linked to low pay. The absence of structured promotion paths also pushed 28% of Customer Support staff and 26% of Sales staff to leave, feeling their efforts led nowhere. Poor onboarding further compounded the problem, with 35% of Customer Support exits and 26% of Sales exits driven by inadequate training and lack of guidance.

Mentorship gaps played a significant role in early exits too, with 450 unmentored Customer Support employees (32%) and 322 unmentored Sales employees (23%) not mentored, highlighting a major void in support for frontline staff.

**Immediate action is required to address pay structure, career advancement opportunities, and onboarding processes** to stabilize the workforce and reduce financial losses.

<br/><br/>

# Insights

### 1. What is the financial impact of employee turnover?

In 2024, Activate Company experienced a dramatic increase in the cost of replacing employees who exited. The total replacement cost in 2024 was **£13,675,148, representing a 493% surge** compared to the previous year’s cost of £2,306,730 in 2023.

<div align = "center">  <img src = "Overall%20Financial%20Impact.png"> </div>

The 2024 replacement cost is over **six times higher than the cost in 2022 which stood at £1,371,216**, 18 times higher than in 2020 when the cost was £543,151. Compared to 2019, when the company spent just £62,364 on replacements, **the 2024 cost is a staggering £13.6 million increase**, marking a 21,825% rise over five years.

<br/>

### 1b. What is the financial impact of employee turnover department-wise?

The combined replacement costs across all departments in 2024 amounted to the overall, **with Customer Support and Sales alone contributing over half of the total cost.**

<div align = "center">  <img src = "Departmental%20Fiancial%20Impact.png"> </div>


* Customer Support: It had the **highest replacement cost** of all departments, reaching £4,420,360. With 314 employees exiting, Customer Support had the largest number of exits too.

* Sales: It was the **second-largest contributor** to replacement costs, with a total of £2,948,638 spent on filling vacant roles.

<br/> 


### 2. What is the rate of turnover in the company?

In 2019, Activate Company had a strong base of long-term employees who had been with the company for over three years. The average tenure across all employees was 5.29 years, indicating a stable workforce with a solid foundation of experienced staff.

<div align = "center">  <img src = "Overall%20turnover.png"> </div>

By 2024, the overall average tenure had **fallen dramatically to just 0.48 years.** The company no longer had a recognizable long-term workforce, and even **the mid-term group had shrunk significantly**, with an average tenure of just 1 year. Early tenure employees, who made up the majority of the workforce, averaged just 0.48 years, showing the **rapid turnover and lack of long-term retention.**

 <br/>

### 2b. What is the turnover rate across departments

The Finance, Customer Support, and IT departments all reported a turnover rate of **20%** and Sales, **16%**.

<div align = "center">  <img src = "Departmental%20Turnover.png"> </div>


 <br/>

### 3. Why are employees leaving rapidly?

* In 2024, **negative feedback dominated exit interviews across all departments**. Employees who left were consistently dissatisfied, expressing concerns that cut across various areas of the organization.

<div align = "center">  <img src = "Exit%20Interview%20Sentiment.png"> </div>

<br/><br/>

* **No Mentorship Participation:** 2024 data shows that the departments with the highest number of employees who were not called to participate in mentorship programs aree **Customer Support and Sales**. These two departments also have the **highest number of exits and the highest replacement costs**, showing a strong link between poor mentorship and high turnover.
<div align = "center">  <img src = "No%20Mentorship%20Reason.png"> </div>
In Customer Support alone, 450 employees, which is 32% of all unmentored staff, did not have a mentor. Sales followed closely, with 322 employees (about 23% of unmentored staff) missing out on mentorship. 

<br/><br/>

* **Low salary** emerged as one of **the most common reasons employees shared for leaving** Activate Company in 2024. Across all departments, staff members expressed frustration over pay that did not meet their expectations or align with industry standards.
<div align = "center">  <img src = "Low%20Salary%20Reason.png"> </div>
The most affected department was Customer Support, where 35% of employees who exited cited low salary as the primary reason for leaving.

<br/><br/>

* **No Promotion Path:** Another key reason employees left Activate Company is the **lack of a clear path for promotion**. Many staff members who exited in 2024 said they did not see opportunities to grow or move up in their careers.
<div align = "center">  <img src = "No%20Promotion%20Path%20Reason.png"> </div>
Customer Support had the highest number of exits for this reason, with 28% of the employees who left saying they saw no promotion path. Sales, with 26% of exited employees in the department also cited the same.

<br/><br/>

* **Poor Onboarding:** Many in 2024 also said they were not properly supported when they first joined. This problem was a common reason for exit, especially in departments that deal directly with customers (Customer Support Staffs and Sales People). 
<div align = "center">  <img src = "Poor%20Onboarding.png"> </div>

<br/><br/>



# Recommendations

* **Always receive ongoing feedback from current employees (Quarterly?)**

* **Address pay discrepancies in departments, especially Customer Support**: Conduct a market salary analysis to align compensation with industry standards, focusing on frontline roles in Customer Support and Sales.

* **Develop clear promotion pathways across all departments**: Create structured career paths that outline specific milestones and promotion criteria for each role, especially in Customer Support and Sales. Establish quarterly reviews for mid-level employees to discuss career progression, identify skill gaps, and provide targeted training opportunities.

* **Revamp onboarding programs with department-specific focus**

* **Strengthen Mentorship Programs to Support Frontline Staff**: Establish a mandatory mentorship program in Customer Support and Sales. Pair new hires with experienced staff to provide ongoing guidance.


# Key questions for stakeholders prior to project advancement

* Beyond exit interviews, has the company collected ongoing feedback from current employees regarding their satisfaction with pay, promotion paths, and mentorship opportunities?

* **Long-Term Workforce Planning**: With the long-term employees phased out, what is the long-term vision for building a more stable, experienced workforce? Are stakeholders open to developing tailored retention plans for critical roles to prevent further losses?








<br/>


Go to my [github homepage](https://github.com/Blessing336)




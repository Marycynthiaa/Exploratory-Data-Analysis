# Explanatory Analysis of Manufacturing Downtime

## Project Background
A manufacturing facility producing six different beverage products has been experiencing significant production downtime, which has negatively impacted overall productivity levels.
The facility’s Operations Manager is responsible for recording key details during these downtime events, including timestamps, the responsible operator, and relevant contextual notes.

This project conducts a comprehensive analysis of the various potential causes of downtime, using explanatory analytics to uncover actionable insights and provide a clear narrative that addresses the central question: “Why is this happening?”

**Key Areas to Note:**
- **Operator Level Analysis:** detailed evaluation of operator-related factors, individual efficiency levels, performance metrics, and the frequency of operator-induced downtime.

- **Pareto Principle Application (80/20 Rule):** Leveraging the Pareto method to identify and prioritize the most critical root causes contributing to prolonged downtime, enabling a focused approach to problem-solving.

- **Product Specific Downtime Assessment:** Analyzing production data to determine whether certain products are more prone to causing downtime, and uncovering the underlying reasons behind these product-related inefficiencies.

## Data Structure
### Data Cleaning 

Because the dataset came in different excel sheets, the first step was to pivot the fields using Index & Match in **Excel** to have relevant information all in one place.
The next step was to look at the provided dataset to ascertain key fields that would be important to this analysis and check for data types. 
The time fields were converted into a *'time'* Data type as ignoring this would give inaccurate or inconsistent results during visualization. Time too for the purpose of this report is calculated in minutes.
Finally, Power query in **Power BI** was used to perform quality checks.
Below is the structure of the dataset in the modeling view:


<img width="500" height="500" alt="MODEL VIEW" src="https://github.com/user-attachments/assets/22f0da5c-f5eb-4202-aed8-6bca3ca9e6a0" /> <img width="700" height="200" alt="Data cleaning" src="https://github.com/user-attachments/assets/c7fac12e-aef1-43ea-bd94-7c11c789b93a" />

## Executive Summary
### Overview of Findings

The analysis revealed that operator related issues are a major contributor to downtime, accounting for a significant portion of total production delays. Using the Pareto Principle, it was discovered that just a few key factors ranging from operator related (Machine adjustment, Batch change, Batch coding error) and system-related (Machine failure, Inventory shortage) are responsible for the majority of downtime events. Product level assessment showed that certain drink types, particularly CO based products (2L and 600ml), are linked to higher adjustment times and setup related delays. These insights highlight clear opportunities for process improvement, targeted training, and product specific setup optimization.

### Operator Level Analysis

An analysis of operational data revealed that **three** of the top **five** causes of downtime are directly linked to operator actions, accounting for **637** minutes *(approximately 11 hours)* of lost time over a 4-day period. If unaddressed, this operator-driven downtime could lead to extended production cycles, reduced throughput, and a negative impact on overall equipment effectiveness **(OEE)**. It also risks compounding operational inefficiencies across shifts and production lines.

- Operator related issues directly reduce availability and performance, two critical components of OEE. Over time, this diminishes line efficiency and increases cost per unit.
- Persistent inefficiencies can lead to operator frustration, burnout, or disengagement which will further degrade performance over time. Although the recurrence of these operator linked issues may indicate gaps in training, standard operating procedures **(SOPs)**, or shift-level accountability in the facility.

<img width="453" height="127" alt="Operator related Table" src="https://github.com/user-attachments/assets/b4eaf8e6-ad2f-4750-8148-c0896cd38b94" />

### The Pareto Principle
The Pareto Principle is known as the 80/20 rule. And it suggests that roughly **80%** of effects come from **20%** of the causes. In clear terms, it implores that a small portion of efforts or causes can lead to a large portion of results or in our case, consequences. 
Using the Pareto Principle to analyze downtime, the chart below highlights the top five contributing factors, all of which fall above the 80% cumulative threshold. A *DAX* was used to calculate the % of total downtime and to generate a cumulative percentage in descending order of impact. The chart below shows us other causes amongst the top 5.

- The analysis revealed that machine adjustment alone accounts for approximately **43%** of total downtime, making it the most significant factor. This is closely followed by machine failure, which also contributes a substantial share showing a result that aligns with common expectations in a manufacturing environment.

<img width="365" height="185" alt="Cumulative DT%" src="https://github.com/user-attachments/assets/91755839-538a-4534-b222-89b9e89f4d97" />

<img width="292" height="166" alt="Cumulative table" src="https://github.com/user-attachments/assets/95046c0d-a8ec-4661-85f8-0a530f8d0a0c" />

### Product Production Assessment
A product level analysis revealed that **43%** of all machine adjustment related downtime occurs during the production of **‘CO’** based products, specifically **CO-2L** (2-litre) and **CO-600** (600ml) variants. This concentration suggests that the handling or setup process for these variants may be improperly configured, leading to extended machine adjustment times.

- These inefficiencies delay production but also contribute to increased mechanical strain, thereby raising the risk of machine failure during or after these production runs.

<img width="325" height="221" alt="decomposition tree" src="https://github.com/user-attachments/assets/37e5e773-d887-4cc1-9f0c-4ce90be4b20e" />

<img width="228" height="231" alt="CO DT description" src="https://github.com/user-attachments/assets/b1c31cd4-07fa-4636-aee1-4f227396deb9" />

## Recommendations
Based on the Insights uncovered, the following recommendations have been provided:

- It is best to Train operators and asses how they handle these machinery/equipment by creating simple, pre-startup and pre-changeover checklists. Focus operators training on specific recurring issues: e.g., machine adjustment, batch change over, and coding setup.
- The focus areas should be on Machine adjustment to standardize settings. Improve Inventory control and Increase alert to notify when there is low stock. And finally validate coding systems and carry out quality assurance checks to reduce batch coding error. If even the top 2 are fixed, 50% of the issues can be eliminated.
- The CO flavour can be less familiar to operators to run leading to more downtime so ensure that setup instructions for CO-2L and CO-600 are clear, updated, and visually accessible at the machine e.g using visual aids. 
- Reinforce pre checks before running the CO based products especially and since this is an operator specific issue, provide digital setup checklist tailored to various errors encountered.

**DAX**
``` data analysis expression
Cumulative % DT = 
VAR Currentfactor = SELECTEDVALUE('Line downtime'[Description])
VAR Rankedtable = ADDCOLUMNS(
        ALL('Downtime factors'[Description]), "Total Downtime", CALCULATE(SUM('Line downtime'[Downtime Minutes])))
VAR sortedtable = ADDCOLUMNS(
        RankedTable, "Rank", RANKX(RankedTable,
        [Total Downtime],,ASC,Dense))
VAR CurrentRank = MAXX(FILTER(sortedtable, [Description] = Currentfactor), [Rank])
VAR Cumulativedowntime = SUMX(
        FILTER(sortedtable, [Rank]<= CurrentRank),[Total Downtime])
VAR Totaldowntime = 
    CALCULATE(SUM('Line downtime'[Downtime Minutes]),ALL('Line downtime'))
RETURN 
        DIVIDE(
            Cumulativedowntime, Totaldowntime)]
```
**Tools Used**
- Excel: Data Cleaning
- Power Query: Merging queries
- Power BI: Data Visualization

**Report Snapshots**

<img width="675" height="381" alt="Pareto Report" src="https://github.com/user-attachments/assets/987e0956-5ef4-4de6-9bd5-487df961e465" />

<img width="676" height="379" alt="Operator Report" src="https://github.com/user-attachments/assets/7cb46a6e-0fa2-4b2d-9b27-4b879631c9a4" />

<img width="676" height="380" alt="Product level Report" src="https://github.com/user-attachments/assets/4d4dc806-5088-4d50-8369-2e19510750e0" />























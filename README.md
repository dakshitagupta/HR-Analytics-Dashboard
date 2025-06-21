# HR-Analytics-Dashboard
This project is an interactive HR Analytics Dashboard built using Power BI. It helps visualize and analyze employee data such as attrition, department-wise distribution, job satisfaction, and performance ratings. The dashboard enables HR teams to make informed, data-driven decisions efficiently.

## Project Objectives

- Provide HR professionals with an at-a-glance overview of employee metrics.
- Enable trend analysis for attrition, satisfaction, and promotions.
- Help decision-makers understand workforce distribution and performance.
- Create an interactive, easy-to-use interface for HR insights.

## Key Features

- Overall employee statistics: headcount, gender ratio, and average age.
- Attrition trends by department, gender, and job role.
- Performance insights: rating distribution and promotion rates.
- Department-wise breakdown of satisfaction and overtime.
- Interactive slicers for age, education, gender, and department.

## Data Cleaning and Preparation

Data was initially in Excel and cleaned using Power Query in Power BI:

- Removed null values and duplicate rows.
- Renamed columns for clarity (e.g., "EmpID" → "EmployeeID").
- Converted data types: Date (Date), Age (Whole Number), etc.
- Standardized values (e.g., "Sales " → "Sales").
- Created custom age group column for demographic segmentation.

### Power Query Transformations

- `Text.Trim([Department])`
- `Table.RemoveRowsWithErrors`
- `Table.Distinct`
- `Added Conditional Columns` for age group and satisfaction category.

## DAX Measures and Calculated Columns

### Basic Measures

```dax
Total Employees = COUNT('HR_Data'[EmployeeID])

Average Age = AVERAGE('HR_Data'[Age])

Attrition Rate (%) = 
DIVIDE(
    CALCULATE(COUNTROWS('HR_Data'), 'HR_Data'[Attrition] = "Yes"),
    COUNTROWS('HR_Data')
)

Average Performance Rating = AVERAGE('HR_Data'[PerformanceRating])

Promotion Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('HR_Data'), 'HR_Data'[PromotionLast5Years] = 1),
    COUNTROWS('HR_Data')
)

Average Satisfaction by Department = 
CALCULATE(
    AVERAGE('HR_Data'[JobSatisfaction]),
    ALLEXCEPT('HR_Data', 'HR_Data'[Department])
)
Age Group = 
SWITCH(TRUE(),
    'HR_Data'[Age] <= 25, "18-25",
    'HR_Data'[Age] <= 35, "26-35",
    'HR_Data'[Age] <= 45, "36-45",
    'HR_Data'[Age] <= 55, "46-55",
    "55+"
)



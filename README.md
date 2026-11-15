# HR Analytics Dashboard Using Power BI

## 1. Project Overview

The HR Analytics Dashboard is a data analytics project developed using Microsoft Power BI. The main purpose of this project is to analyze employee information and generate meaningful insights into workforce distribution, employee attrition, engagement, compensation, promotion eligibility, retrenchment risk, and employee risk levels.

The project converts raw HR data from Excel into an interactive Power BI report. It uses Power Query for data cleaning and transformation, DAX for calculations and KPI development, and Power BI visualizations to present the analysis.

The report also includes bookmark-based navigation, interactive slicers, filter reset functionality, and employee-level drill-through.

## 2. Project Objectives

- Analyze the total number of employees and active employees.
- Identify employee attrition patterns.
- Understand employee satisfaction and engagement.
- Analyze workforce distribution across departments and job roles.
- Examine employee compensation and salary distribution.
- Identify employees due for promotion.
- Analyze employees flagged for retrenchment.
- Calculate employee risk scores and classify employees into risk categories.
- Provide detailed employee information through drill-through functionality.

## 3. Tools and Technologies

| Technology | Purpose |
|---|---|
| Microsoft Excel | Source data storage |
| Power BI Desktop | Report development and data visualization |
| Power Query Editor | Data cleaning and transformation |
| DAX | Calculated columns, measures, and KPIs |
| Power BI Bookmarks | Navigation between report sections |
| Drill-Through | Employee-level detailed analysis |

## 4. Dataset Description

The project uses the `HR Analytics Data.xlsx` workbook.

The workbook contains four sheets:

**1. HR Sheet**

Contains the main employee information, including department, job role, age, gender, salary, satisfaction attributes, attrition, overtime, business travel, and other HR-related details.

**2. Employees Sheet**

Contains EmployeeNumber and Employee Name. This information is used to identify employees.

**3. Promotion Sheet**

Contains Employee Name and Due for Promotion. It identifies employees who are due for promotion.

**4. Retrenchment Sheet**

Contains Employee Name and Will Be Retrenched. It identifies employees flagged for retrenchment.

The main HR dataset contains 1,470 employee records.

## 5. Data Cleaning and Transformation Using Power Query

Power Query Editor was used to prepare the source data before creating the report.

The following operations were performed:

### 5.1 Data Import

The Excel workbook was imported into Power BI using the Excel Workbook connector.

The required sheets were selected and loaded into Power Query Editor for transformation.

### 5.2 Data Inspection

The columns and records were examined to understand the dataset structure.

Data types, column names, missing values, and employee-related attributes were checked before performing transformations.

### 5.3 Data Cleaning

Data preparation included checking and handling null values, reviewing column data types, and preparing the required employee attributes.

The promotion information was also checked, and missing promotion status values were handled using the agreed status representation.

### 5.4 Data Integration

The employee information from the different sheets was combined to create a consolidated employee-level table named `Employee_Master`.

EmployeeNumber was used as the employee identifier, while Employee Name was used to match the employee information from the promotion and retrenchment sheets.

The resulting table provides the employee information required for the dashboard.

### 5.5 Final Employee Master Table

The final `Employee_Master` table contains 1,470 employee records and serves as the primary employee-level data source for the report.

It includes employee information, workforce attributes, satisfaction details, promotion status, retrenchment status, and risk-related fields.

## 6. Data Modeling

Data modeling is an important step because it organizes the transformed data for analysis.

The project uses the consolidated `Employee_Master` table as its main employee-level table.

**EmployeeNumber** is used as the unique employee identifier for employee counting and employee-level analysis.

The employee table provides the information used to calculate:

- Total and active employees
- Attrition employees
- Employee satisfaction
- Department-wise workforce distribution
- Salary analysis
- Promotion eligibility
- Retrenchment flags
- Employee risk scores

Using an employee-centric table helps maintain consistent employee-level analysis across the report.

## 7. DAX Calculations and KPI Development

DAX (Data Analysis Expressions) is used to create calculations and measures in Power BI.

These calculations help generate meaningful HR metrics from the employee data.

### 7.1 Total Employees

```dax
Total Employees =
DISTINCTCOUNT(Employee_Master[EmployeeNumber])
```

**Purpose:** Counts unique employees using EmployeeNumber.

**Result:** 1,470 employees.

### 7.2 Active Employees

Active employees are calculated by counting employees whose Attrition status is No.

```dax
Active Employees =
CALCULATE(
    [Total Employees],
    Employee_Master[Attrition] = "No"
)
```

**Purpose:** Identifies employees who are currently active according to the dataset.

**Result:** 1,233 employees.

### 7.3 Attrition Employees

Attrition employees are identified using the Attrition column.

```dax
Attrition Employees =
CALCULATE(
    [Total Employees],
    Employee_Master[Attrition] = "Yes"
)
```

**Purpose:** Counts employees marked as having left the organization.

**Result:** 237 employees.

### 7.4 Attrition Rate

```dax
Attrition Rate =
DIVIDE(
    [Attrition Employees],
    [Total Employees],
    0
) * 100
```

**Purpose:** Calculates the percentage of employees marked with Attrition = Yes.

**Result:** 16.12%.

### 7.5 Employees Due for Promotion

Promotion eligibility is identified using the Due for Promotion flag.

The calculation counts employees whose promotion status is positive.

**Result:** 72 employees.

### 7.6 Employees at Retrenchment Risk

The Will Be Retrenched flag is used to identify employees marked for retrenchment.

The calculation counts employees with a positive retrenchment flag.

**Result:** 117 employees.

### 7.7 Retrenchment Risk Rate

The retrenchment risk rate is calculated using the number of employees flagged for retrenchment divided by the total employee count.

**Result:** 7.96%.

### 7.8 Employee Risk Score

A rule-based risk score is calculated using five selected employee attributes.

Each applicable condition contributes one point.

| Condition | Score |
|---|---:|
| Overtime = Yes | 1 |
| Job Satisfaction less than or equal to 2 | 1 |
| Environment Satisfaction less than or equal to 2 | 1 |
| Work-Life Balance less than or equal to 2 | 1 |
| Business Travel = Travel_Frequently | 1 |

The employee's total risk score is calculated by adding the applicable points.

### 7.9 Risk Category

The risk score is converted into categories using the following conditions:

| Risk Score | Risk Category |
|---|---|
| 0–1 | Low |
| 2 | Medium |
| 3–4 | High |
| 5 | Critical |

### 7.10 High Risk Employees

Employees with a risk score greater than or equal to 3 are classified as High Risk Employees. This includes both High and Critical categories.

**Result:** 240 employees.

### 7.11 Promotion–Retrenchment Conflict

A separate conflict condition identifies employees who have both:

- Due for Promotion = 1
- Will Be Retrenched = 1

**Result:** 54 employees.

These records are highlighted for further HR review.

## 8. Power BI Report Development

After data preparation and DAX calculations, the report was developed using different visualizations and interactive features.

The report is organized into analytical sections using bookmark-based navigation, with a separate Employee Detail drill-through page.

### 8.1 HR Analysis Overview

Provides a high-level view of important HR metrics and employee attributes.

It includes department-wise comparisons of retention rate and attrition rate.

### 8.2 HR Executive Dashboard

Displays important KPI cards and department-wise HR analysis.

The dashboard combines employee count, active employees, attrition rate, average income, promotion eligibility, and retrenchment-risk information.

### 8.3 Employee Engagement Analysis

Uses employee satisfaction attributes to analyze engagement.

Users can explore Environment Satisfaction, Job Satisfaction, Relationship Satisfaction, and Work-Life Balance.

The report also displays employee counts by satisfaction level and the average engagement score.

### 8.4 Workforce and Compensation Analysis

Analyzes employee distribution across departments, salary bands, gender, job roles, years at company, and business travel categories.

It helps understand the workforce structure and compensation patterns.

### 8.5 Performance and Promotion Analysis

Analyzes promotion eligibility across departments and job levels.

It also examines promotion rates and the number of years since the last promotion.

### 8.6 Retrenchment Analysis

Uses the retrenchment flag to identify employees marked for retrenchment.

A decomposition tree is included to break down the retrenchment-risk count by department and other available categories.

### 8.7 Employee Risk Analysis

Displays the calculated risk score, risk category, and employee risk distribution.

The report helps identify employees classified as Low, Medium, High, and Critical risk.

### 8.8 Raw Employee Data

Provides a detailed employee-level table containing employee information, HR attributes, promotion status, retrenchment status, and risk details.

This section also acts as the starting point for employee-level drill-through analysis.

## 9. Interactive Features and Report Navigation

The report uses interactive functionality to make analysis easier.

### 9.1 Bookmark Navigation

Power BI bookmarks are used to navigate between the analytical sections within the main report page.

Navigation buttons and icons are connected to bookmarks.

When a user selects a navigation icon, the corresponding report section is displayed.

This allows different analytical views to be accessed through a single navigation interface.

### 9.2 Slicers

Slicers are used to filter the report based on employee attributes.

The report includes filters for relevant attributes such as Department, Job Role, Job Level, Gender, Age Band, Overtime, Business Travel, and satisfaction levels.

When a user selects a filter value, the related visuals and calculations respond according to the filter context.

### 9.3 Clear Filters

The Clear Filters functionality allows users to remove selected filter values and return to the unfiltered view for the relevant slicers.

### 9.4 Reset Dashboard

The Reset Dashboard button restores the report to its predefined default view using the configured reset functionality.

### 9.5 Interactive Visual Analysis

Users can select data points in charts and explore the related information in other report visuals, depending on the visual interactions configured in Power BI.

## 10. Employee Detail – Drill-Through Functionality

Drill-through is implemented to provide detailed information about an individual employee.

A separate Employee Detail page is configured to receive the selected employee context from the Raw Data table.

### 10.1 How Drill-Through Works

1. Navigate to the Raw Data section.
2. Select an employee record.
3. Right-click the employee record.
4. Choose the Drill through option.
5. Select the Employee Detail page.
6. Power BI navigates to the detail page for the selected employee.

### 10.2 How Employee Details Are Filtered

The selected employee is passed to the Employee Detail page through the drill-through field configuration.

The employee context filters the information displayed on that page.

This means the page displays details for the selected employee instead of showing the complete employee dataset.

### 10.3 Employee Summary

The page displays the selected employee's:

- Employee Name
- Employee Number
- Department
- Job Role
- Risk Score

### 10.4 Employee Information

The detailed information includes:

- Age
- Gender
- Job Involvement
- Monthly Income
- Business Travel
- Overtime
- Attrition
- Due for Promotion
- Will Be Retrenched

### 10.5 Employee Experience and Performance Details

The page also displays available experience and performance attributes:

- Years at Company
- Years in Current Role
- Years Since Last Promotion
- Job Satisfaction
- Environment Satisfaction
- Relationship Satisfaction
- Work-Life Balance
- Performance Rating
- Risk Category

### 10.6 Back Navigation

A Back button is provided on the Employee Detail page.

When the user selects this button, Power BI returns to the previous report view.

This functionality makes it easier to move between overall workforce analysis and individual employee investigation.

## 11. Data Validation and Testing

After developing the report, the KPI calculations and interactive features were checked.

The main employee count and calculated metrics were compared with the expected dataset results.

**Validated KPI Results:**

| Metric | Validated Result |
|---|---:|
| Total Employees | 1,470 |
| Active Employees | 1,233 |
| Attrition Employees | 237 |
| Attrition Rate | 16.12% |
| Employees Due for Promotion | 72 |
| Employees at Retrenchment Risk | 117 |
| High Risk Employees | 240 |
| Promotion–Retrenchment Conflicts | 54 |

**Risk Category Validation:**

| Risk Category | Employee Count |
|---|---:|
| Low | 747 |
| Medium | 483 |
| High | 238 |
| Critical | 2 |
| Total | 1,470 |

The following report features were also checked:

- Bookmark navigation
- Department and employee attribute slicers
- Clear Filters
- Reset Dashboard
- Raw Data table
- Employee Detail drill-through
- Back navigation

## 12. Key Business Findings

The analysis provides the following findings:

1. The dataset contains 1,470 employees.
2. There are 1,233 active employees.
3. A total of 237 employees are marked as having left the organization.
4. The attrition rate is 16.12%.
5. A total of 72 employees are identified as due for promotion.
6. A total of 117 employees are flagged for retrenchment.
7. A total of 240 employees fall under High or Critical risk categories.
8. There are 54 employees with both promotion and retrenchment flags.

These findings can help HR teams identify areas requiring further investigation and review.

## 16. Project Demonstration Video

The project demonstration video explains the report development outcome and shows how the different analytical sections and interactive features work.

**Video:** "C:\Users\boddu\Videos\Screen Recordings\Screen Recording 2026-11-15 090639.mp4"

## 17. Technologies Used

- Microsoft Power BI
- Power Query Editor
- DAX
- Microsoft Excel


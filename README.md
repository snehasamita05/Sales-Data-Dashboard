Step 1: Understand the Data
Identify the relationships between Customer Data, Product Data, and Sales Transactions tables.
Ensure the data model has appropriate primary and foreign keys:
Customer Data → Customer ID (links to Sales Transactions)
Product Data → Product ID (links to Sales Transactions)
Sales Transactions → Contains sales data with dates, amounts, regions, etc.
Step 2: Prepare the Data
Import Data:

Load the provided datasets into Power BI.
Use Power Query to clean and format the data:
Ensure data types are correct (e.g., dates, numbers, text).
Remove unnecessary columns if needed.


Ensure Correct Data Types:

For each column, check its data type in the Data Type column on the left.
If the data type is incorrect, click on the Data Type icon next to the column name and choose the correct type:
Date/Time: For any columns containing date values (e.g., Order Date, Invoice Date).
Whole Number: For any integer columns (e.g., Quantity, CustomerID).
Decimal Number: For numeric values with decimals (e.g., Invoiced Amount, Sales Growth %).
Text: For columns containing strings (e.g., Product Name, Customer Type).
Ensure all the columns with date values are correctly formatted as Date/Time, especially if you are performing time-based calculations.
Remove Unnecessary Columns:

Identify and remove any columns that are not required for the analysis. For instance:
Customer data might contain columns like Address or Phone Number, which may not be needed for this task.
Product data could have columns like Supplier Information, which are not relevant.
To remove columns:
Select the column(s) you want to remove.
Right-click on the column header and choose Remove.
Alternatively, you can click on the Remove Columns button in the ribbon.




Create a Data Model:

Use the Model view to establish relationships between tables.
Ensure the model supports the required hierarchy:
Sales Region Hierarchy: Region → Country → City
Product Hierarchy: Category → Subcategory → Product
Add a Date Table:

Create or import a Date Table for time intelligence functions.
Mark the Date Table as the Date Table in Power BI.


Create or Import a Date Table: Use DAX to generate a Date Table or import from an external source.
Add Columns: Add additional columns like Year, Month, Quarter, etc., for better time-based analysis.
Mark as Date Table: Ensure that Power BI recognizes the Date Table by marking it as the Date Table.
Create Relationships: Link the Date Table to other tables using date columns.
Use Time Intelligence: Use DAX time functions (YTD, MTD, etc.) for analysis.



Step 3: Develop Visuals
1. Total Invoiced Amount YTD vs Last YTD
Use Measure to calculate these:
Total Invoiced Amount YTD = CALCULATE(SUM(Sales[Net Price]), DATESYTD(Date[Date]))
Total Invoiced Amount Last YTD = 
CALCULATE(
    SUM(Sales[Net Price]),
    DATESYTD(DATEADD(DateTable[Date], -1, YEAR))
)

2. % Sales Growth YTD with Thresholds
Create a measure for sales growth:
% Sales Growth YTD = DIVIDE([Total Invoiced Amount YTD] - [Total Invoiced Amount Last YTD], [Total Invoiced Amount Last YTD]) * 100
Add conditional formatting for green, yellow, and red:
Use thresholds: Green (>5%), Yellow (0-5%), Red (negative).
3. Map Visualization
Use the Map or Filled Map visualization.
Display Sales Growth % by country.
Use conditional formatting to reflect the thresholds.
4. Daily Sales Report
Create a line chart:
X-Axis: Date (filtered to the selected month).
Y-Axis: Ordered Amount vs. Invoiced Amount.
Use slicers to filter the selected month.
5. Month-by-Month Comparison
Use a clustered column chart:
X-Axis: Month Name (use a date hierarchy).
Y-Axis: Actual vs. Previous Year Invoiced Amount.
Create a measure for the previous year:
Previous Year Invoiced = CALCULATE(SUM(Sales[Invoiced Amount]), SAMEPERIODLASTYEAR(Date[Date]))
6. New Customers
Create a measure to count new customers:
New Customers = COUNTROWS(FILTER(Customers, MINX(RELATED(Sales), Sales[Transaction Date]) >= DATE(YEAR([Selected Date]), 1, 1)))
7. Top 5 Customers
Use a table visual:
Columns: Customer Name, Order Amount.
Add a Top N filter (set N = 5).
Step 4: Add Filters
Single-Selection Filter for Customer Type:

Add a slicer for "Type of Customer."
Enable Single Selection in the slicer settings.
Drill-Down Filters:

For Product Hierarchy and Sales Region Hierarchy, use a hierarchy slicer or visualizations with drill-down capabilities.
Date Picker:

Use a slicer with the Date field to allow selection of a specific date.
Step 5: Configure Slicer Interactions
Go to the Format tab → Edit Interactions:
Configure slicers to ensure they interact dynamically.
For example, selecting a region updates available countries and cities.
Step 6: Design the Report
Arrange visuals to fit on one page.
Use a clean, professional theme.
Add tooltips to provide additional insights (e.g., details on % growth).
Step 7: Validate and Publish
Test the report by selecting different filters and slicers.
Validate calculations to ensure accuracy.
Publish the report to Power BI Service and configure appropriate Row-Level Security (RLS):
Restrict regions to their respective sales units while allowing high-level managers to see all data.

- North America: United States + Canada

- West Europe: France, Germany, Italy, Ireland, Portugal, Spain, Switzerland, the Netherlands and United Kingdom

- All Others

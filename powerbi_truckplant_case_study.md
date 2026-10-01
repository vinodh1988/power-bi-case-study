# Power BI Case Study: Truck Plant Dataset Analysis

## Objective

Create a detailed Power BI report for the truck manufacturing dataset using simple, native Power BI features. The report should help business users analyze production, sales, service, dealers, customers, truck models, and customer feedback without writing complex DAX.

Use only:

- CSV import
- Power Query data type cleanup
- Basic relationships
- Standard Power BI visuals
- Built-in visual aggregations such as Sum, Average, Count, Distinct count, Minimum, and Maximum
- Simple slicers and filters
- Drill-down on dates and categories
- Optional drill-through pages
- Optional report page tooltips
- Conditional formatting in tables and matrices

Avoid:

- Complex DAX measures
- Advanced calculated tables
- Complex calculated columns
- Python visuals
- R visuals
- Power BI Copilot
- Q&A visual
- Smart Narrative visual
- Key Influencers visual
- AI Insights
- Text analytics
- Machine learning models
- External scripts

## Dataset Files

| File | Purpose |
|---|---|
| `Customers.csv` | Customer details such as industry, region, company size, fleet size, customer type, and account start date |
| `Trucks.csv` | Truck details such as model, truck type, engine type, fuel type, plant, capacity, warranty, and base price |
| `Dealers.csv` | Dealer details such as region, dealer type, staff count, opening date, and total sales indicator |
| `DateTable.csv` | Calendar table for year, quarter, month, week, and day analysis |
| `Production.csv` | Production records by truck, plant, date, quantity, cost, shift, supervisor, and machine line |
| `Sales.csv` | Sales transactions by truck, customer, dealer, date, price, channel, region, and payment type |
| `Service.csv` | Service records by truck, dealer, date, service type, cost, feedback score, downtime, and part replaced |
| `CustomerFeedback.csv` | Customer feedback by customer, truck, date, rating, satisfaction, recommendation score, and comment |

## Business Questions

The report should answer these questions:

1. Which truck types and models generate the most sales revenue?
2. Which regions and dealers have the strongest sales performance?
3. Which plants produce the most trucks?
4. Which shifts and machine lines are most active?
5. Which truck types or plants have higher production cost?
6. Which customer industries and company sizes buy the most trucks?
7. Which service types happen most often?
8. Which replaced parts create the highest service cost or downtime?
9. Which truck types receive the best customer ratings?
10. Which dealers or truck models need follow-up because sales are strong but service or feedback is weak?

## Import Steps

1. Open Power BI Desktop.
2. Select `Get Data` > `Text/CSV`.
3. Import these 8 files:
   - `Customers.csv`
   - `Trucks.csv`
   - `Dealers.csv`
   - `DateTable.csv`
   - `Production.csv`
   - `Sales.csv`
   - `Service.csv`
   - `CustomerFeedback.csv`
4. Select `Transform Data`.
5. Confirm that Power BI has used the first row as headers.
6. Set data types in Power Query.
7. Select `Close & Apply`.

## Data Type Setup

Use these simple rules:

| Field Type | Power BI Data Type | Examples |
|---|---|---|
| ID columns | Whole Number | `CustomerID`, `TruckID`, `DealerID`, `SaleID` |
| Date columns | Date | `SaleDate`, `ProductionDate`, `ServiceDate`, `FeedbackDate` |
| Money columns | Decimal Number | `SalePrice`, `BasePrice`, `ProductionCost`, `ServiceCost` |
| Quantity and score columns | Whole Number | `QuantityProduced`, `FleetSize`, `Rating`, `DowntimeDays` |
| Names and categories | Text | `TruckType`, `Region`, `DealerName`, `Comments` |

After changing data types, scan each query for obvious errors. If any column shows conversion errors, correct the data type before loading.

## Relationship Setup

Open Model view and create these relationships.

| Table | Column | Related Table | Related Column | Type |
|---|---|---|---|---|
| `DateTable` | `Date` | `Sales` | `SaleDate` | One-to-many |
| `DateTable` | `Date` | `Production` | `ProductionDate` | One-to-many |
| `DateTable` | `Date` | `Service` | `ServiceDate` | One-to-many |
| `DateTable` | `Date` | `CustomerFeedback` | `FeedbackDate` | One-to-many |
| `Trucks` | `TruckID` | `Sales` | `TruckID` | One-to-many |
| `Trucks` | `TruckID` | `Production` | `TruckID` | One-to-many |
| `Trucks` | `TruckID` | `Service` | `TruckID` | One-to-many |
| `Trucks` | `TruckID` | `CustomerFeedback` | `TruckID` | One-to-many |
| `Customers` | `CustomerID` | `Sales` | `CustomerID` | One-to-many |
| `Customers` | `CustomerID` | `CustomerFeedback` | `CustomerID` | One-to-many |
| `Dealers` | `DealerID` | `Sales` | `DealerID` | One-to-many |
| `Dealers` | `DealerID` | `Service` | `DealerID` | One-to-many |

Recommended settings:

- Cardinality: One-to-many.
- Cross filter direction: Single.
- Keep all four fact tables separate: `Sales`, `Production`, `Service`, and `CustomerFeedback`.
- Mark `DateTable` as the date table using `Table tools` > `Mark as date table` > choose `Date`.

## Simple Calculations Only

This report can be built without creating formal DAX measures. Use visual-level aggregation instead.

In each visual, Power BI can summarize numeric columns automatically:

| Business Metric | How to Create It in a Visual |
|---|---|
| Sales revenue | Drag `Sales[SalePrice]` into Values and set aggregation to `Sum` |
| Number of sales | Drag `Sales[SaleID]` into Values and set aggregation to `Count` |
| Average sale price | Drag `Sales[SalePrice]` into Values and set aggregation to `Average` |
| Production quantity | Drag `Production[QuantityProduced]` into Values and set aggregation to `Sum` |
| Production cost | Drag `Production[ProductionCost]` into Values and set aggregation to `Sum` or `Average` |
| Number of production records | Drag `Production[ProductionID]` into Values and set aggregation to `Count` |
| Service cost | Drag `Service[ServiceCost]` into Values and set aggregation to `Sum` or `Average` |
| Number of service events | Drag `Service[ServiceID]` into Values and set aggregation to `Count` |
| Average service score | Drag `Service[FeedbackScore]` into Values and set aggregation to `Average` |
| Average downtime | Drag `Service[DowntimeDays]` into Values and set aggregation to `Average` |
| Feedback count | Drag `CustomerFeedback[FeedbackID]` into Values and set aggregation to `Count` |
| Average rating | Drag `CustomerFeedback[Rating]` into Values and set aggregation to `Average` |
| Average recommendation | Drag `CustomerFeedback[LikelihoodToRecommend]` into Values and set aggregation to `Average` |
| Number of customers | Drag `Customers[CustomerID]` into Values and set aggregation to `Distinct count` |
| Number of truck models | Drag `Trucks[TruckID]` into Values and set aggregation to `Distinct count` |

## Optional Simple Calculated Columns

Calculated columns are optional. Use them only if they make visuals easier to read.

### Customer Account Age Group

Create this in `Customers` if you want to group customers by account age:

```DAX
Account Age Group =
VAR YearsOld = DATEDIFF(Customers[AccountSince], TODAY(), YEAR)
RETURN
SWITCH(
    TRUE(),
    YearsOld <= 2, "0-2 Years",
    YearsOld <= 5, "3-5 Years",
    YearsOld <= 8, "6-8 Years",
    "9+ Years"
)
```

Simpler alternative: skip this column and use `Customers[AccountSince]` directly in visuals or filters.

### Truck Price Band

Create this in `Trucks` if you want easy price grouping:

```DAX
Price Band =
SWITCH(
    TRUE(),
    Trucks[BasePrice] < 75000, "Low",
    Trucks[BasePrice] < 120000, "Mid",
    "High"
)
```

### Service Downtime Group

Create this in `Service` if you want simple downtime categories:

```DAX
Downtime Group =
SWITCH(
    TRUE(),
    Service[DowntimeDays] <= 1, "Low",
    Service[DowntimeDays] <= 3, "Medium",
    "High"
)
```

If you want the simplest possible build, do not create these columns. The full report can still be completed using only existing columns and built-in visual aggregations.

## Report Pages

Create these pages:

1. Executive Overview
2. Sales Analysis
3. Production Analysis
4. Dealer and Region Analysis
5. Customer Analysis
6. Service Analysis
7. Feedback Analysis
8. Truck Model Detail
9. Detail Tables

Keep the layout simple:

- Place slicers at the top or left.
- Place KPI cards near the top.
- Place trend charts in the middle.
- Place ranked tables or detail visuals near the bottom.

## Page 1: Executive Overview

### Purpose

Give a quick summary of the whole business.

### Slicers

Add these slicers:

- `DateTable[Year]`
- `DateTable[Quarter]`
- `Sales[Region]`
- `Trucks[TruckType]`
- `Trucks[Plant]`

### KPI Cards

Create cards using these fields and aggregations:

| Card | Field | Aggregation |
|---|---|---|
| Total Sales Revenue | `Sales[SalePrice]` | Sum |
| Sales Transactions | `Sales[SaleID]` | Count |
| Quantity Produced | `Production[QuantityProduced]` | Sum |
| Production Cost | `Production[ProductionCost]` | Sum |
| Service Events | `Service[ServiceID]` | Count |
| Average Rating | `CustomerFeedback[Rating]` | Average |
| Average Service Score | `Service[FeedbackScore]` | Average |
| Average Recommendation | `CustomerFeedback[LikelihoodToRecommend]` | Average |

### Visual 1: Sales Revenue Trend

Visual type: Line chart

- X-axis: `DateTable[Date]`
- Y-axis: `Sales[SalePrice]`, Sum
- Legend: `DateTable[Year]`

Use the date hierarchy to drill from year to quarter to month.

### Visual 2: Revenue by Truck Type

Visual type: Clustered bar chart

- Y-axis: `Trucks[TruckType]`
- X-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Sales[SaleID]`, Count
  - `Sales[SalePrice]`, Average
  - `CustomerFeedback[Rating]`, Average

### Visual 3: Production Quantity by Plant

Visual type: Clustered column chart

- X-axis: `Trucks[Plant]`
- Y-axis: `Production[QuantityProduced]`, Sum
- Tooltips:
  - `Production[ProductionCost]`, Sum
  - `Production[ProductionCost]`, Average

### Visual 4: Service Events by Service Type

Visual type: Column chart

- X-axis: `Service[ServiceType]`
- Y-axis: `Service[ServiceID]`, Count
- Tooltips:
  - `Service[ServiceCost]`, Average
  - `Service[DowntimeDays]`, Average
  - `Service[FeedbackScore]`, Average

### Insights to Capture

- Highest revenue truck type.
- Highest production plant.
- Most common service type.
- Overall average rating and recommendation level.
- Any year or quarter where sales look unusually high or low.

## Page 2: Sales Analysis

### Purpose

Understand where revenue comes from and how sales differ by region, dealer, truck type, channel, and payment method.

### Slicers

- `DateTable[Year]`
- `DateTable[Quarter]`
- `Sales[Region]`
- `Trucks[TruckType]`
- `Sales[SalesChannel]`
- `Sales[PaymentType]`

### Visual 1: Revenue by Region

Visual type: Bar chart

- Y-axis: `Sales[Region]`
- X-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Sales[SaleID]`, Count
  - `Sales[SalePrice]`, Average
  - `Customers[CustomerID]`, Distinct count

Sort descending by Sum of `SalePrice`.

### Visual 2: Monthly Revenue Trend

Visual type: Line chart

- X-axis: `DateTable[Date]`
- Y-axis: `Sales[SalePrice]`, Sum
- Legend: `Sales[Region]`

Use drill-down to compare year, quarter, and month.

### Visual 3: Truck Type by Sales Channel

Visual type: Matrix

- Rows: `Trucks[TruckType]`
- Columns: `Sales[SalesChannel]`
- Values:
  - `Sales[SalePrice]`, Sum
  - `Sales[SaleID]`, Count
  - `Sales[SalePrice]`, Average

Use conditional formatting on Sum of `SalePrice`.

### Visual 4: Payment Type Mix

Visual type: 100% stacked column chart

- X-axis: `Sales[Region]`
- Legend: `Sales[PaymentType]`
- Values: `Sales[SaleID]`, Count

### Visual 5: Top 10 Truck Models by Revenue

Visual type: Bar chart

- Y-axis: `Trucks[ModelName]`
- X-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Trucks[TruckType]`
  - `Trucks[BasePrice]`, Average
  - `Trucks[CapacityTons]`, Average
  - `Sales[SalePrice]`, Average

Use the visual-level Top N filter:

1. Add `Trucks[ModelName]` to visual filters.
2. Change filter type to Top N.
3. Show Top 10 by Sum of `Sales[SalePrice]`.
4. Apply filter.

### Visual 6: Sale Price Bands

Visual type: Column chart

Steps:

1. Right-click `Sales[SalePrice]` in the Fields pane.
2. Select `New group`.
3. Choose bin size such as 10,000.
4. Put the Sale Price bin on the X-axis.
5. Put `Sales[SaleID]`, Count, on the Y-axis.

### Insights to Capture

- Best region by revenue.
- Region with highest number of transactions.
- Whether dealer or online channel performs better.
- Whether cash or financing is more common.
- Top truck models by revenue.
- Whether revenue is concentrated in expensive trucks or spread across price bands.

## Page 3: Production Analysis

### Purpose

Analyze production quantity, production cost, plant activity, shifts, and production lines.

### Slicers

- `DateTable[Year]`
- `DateTable[Quarter]`
- `Production[Plant]`
- `Production[Shift]`
- `Production[MachineUsed]`
- `Trucks[TruckType]`

### Visual 1: Quantity Produced by Plant

Visual type: Column chart

- X-axis: `Production[Plant]`
- Y-axis: `Production[QuantityProduced]`, Sum
- Tooltips:
  - `Production[ProductionID]`, Count
  - `Production[ProductionCost]`, Sum
  - `Production[ProductionCost]`, Average

### Visual 2: Production Cost by Plant

Visual type: Bar chart

- Y-axis: `Production[Plant]`
- X-axis: `Production[ProductionCost]`, Sum
- Tooltips:
  - `Production[QuantityProduced]`, Sum
  - `Production[ProductionCost]`, Average

### Visual 3: Plant and Shift Matrix

Visual type: Matrix

- Rows: `Production[Plant]`
- Columns: `Production[Shift]`
- Values:
  - `Production[QuantityProduced]`, Sum
  - `Production[ProductionCost]`, Sum
  - `Production[ProductionCost]`, Average

Apply conditional formatting to Average of `ProductionCost`.

### Visual 4: Production by Machine Line

Visual type: Stacked bar chart

- Y-axis: `Production[MachineUsed]`
- X-axis: `Production[QuantityProduced]`, Sum
- Legend: `Production[Shift]`

### Visual 5: Production Trend

Visual type: Line chart

- X-axis: `DateTable[Date]`
- Y-axis: `Production[QuantityProduced]`, Sum
- Legend: `Production[Plant]`

### Visual 6: Capacity vs Production Cost

Visual type: Scatter chart

- X-axis: `Trucks[CapacityTons]`, Average
- Y-axis: `Production[ProductionCost]`, Average
- Size: `Production[QuantityProduced]`, Sum
- Legend: `Trucks[TruckType]`
- Details: `Trucks[ModelName]`

### Insights to Capture

- Plant with highest production quantity.
- Plant with highest total production cost.
- Shift with highest activity.
- Machine line with highest production quantity.
- Whether heavier or higher-capacity trucks have higher production cost.

## Page 4: Dealer and Region Analysis

### Purpose

Compare dealers and regions across sales and service performance.

### Slicers

- `Dealers[Region]`
- `Dealers[DealerType]`
- `DateTable[Year]`
- `Sales[SalesChannel]`
- `Service[ServiceType]`

### Visual 1: Dealer Leaderboard

Visual type: Table

Columns and aggregations:

- `Dealers[DealerName]`
- `Dealers[Region]`
- `Dealers[DealerType]`
- `Dealers[StaffCount]`, Average
- `Sales[SalePrice]`, Sum
- `Sales[SaleID]`, Count
- `Sales[SalePrice]`, Average
- `Service[ServiceID]`, Count
- `Service[FeedbackScore]`, Average
- `Service[DowntimeDays]`, Average

Sort by Sum of `Sales[SalePrice]` descending.

### Visual 2: Revenue by Dealer Type

Visual type: Bar chart

- Y-axis: `Dealers[DealerType]`
- X-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Sales[SaleID]`, Count
  - `Sales[SalePrice]`, Average
  - `Service[FeedbackScore]`, Average

### Visual 3: Region and Truck Type Matrix

Visual type: Matrix

- Rows: `Dealers[Region]`
- Columns: `Trucks[TruckType]`
- Values:
  - `Sales[SalePrice]`, Sum
  - `Sales[SaleID]`, Count
  - `CustomerFeedback[Rating]`, Average

### Visual 4: Staff Count vs Revenue

Visual type: Scatter chart

- X-axis: `Dealers[StaffCount]`, Average
- Y-axis: `Sales[SalePrice]`, Sum
- Size: `Sales[SaleID]`, Count
- Legend: `Dealers[DealerType]`
- Details: `Dealers[DealerName]`

### Visual 5: Dealer Service Score

Visual type: Bar chart

- Y-axis: `Dealers[DealerName]`
- X-axis: `Service[FeedbackScore]`, Average
- Tooltips:
  - `Service[ServiceID]`, Count
  - `Service[DowntimeDays]`, Average
  - `Service[ServiceCost]`, Sum

Use Top N or Bottom N filters to show best and weakest dealers.

### Insights to Capture

- Best dealers by sales revenue.
- Dealers with high revenue but lower service score.
- Dealer type with stronger sales.
- Regions with the best sales and ratings.
- Whether staff count appears related to revenue.

## Page 5: Customer Analysis

### Purpose

Analyze which customer groups buy the most and provide the best feedback.

### Slicers

- `Customers[IndustryType]`
- `Customers[CompanySize]`
- `Customers[CustomerType]`
- `Customers[Region]`
- `DateTable[Year]`

### Visual 1: Revenue by Industry

Visual type: Bar chart

- Y-axis: `Customers[IndustryType]`
- X-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Customers[CustomerID]`, Distinct count
  - `Sales[SaleID]`, Count
  - `Sales[SalePrice]`, Average

### Visual 2: Sales by Company Size

Visual type: Column chart

- X-axis: `Customers[CompanySize]`
- Y-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Sales[SaleID]`, Count
  - `Customers[FleetSize]`, Average

### Visual 3: Customer Type Split

Visual type: 100% stacked column chart

- X-axis: `Customers[IndustryType]`
- Legend: `Customers[CustomerType]`
- Values: `Sales[SaleID]`, Count

### Visual 4: Fleet Size vs Revenue

Visual type: Scatter chart

- X-axis: `Customers[FleetSize]`, Average
- Y-axis: `Sales[SalePrice]`, Sum
- Size: `Sales[SaleID]`, Count
- Legend: `Customers[CompanySize]`
- Details: `Customers[CustomerName]`
- Tooltips:
  - `Customers[IndustryType]`
  - `Customers[CustomerType]`
  - `CustomerFeedback[Rating]`, Average

### Visual 5: Top Customers Table

Visual type: Table

Columns:

- `Customers[CustomerName]`
- `Customers[IndustryType]`
- `Customers[Region]`
- `Customers[CompanySize]`
- `Customers[FleetSize]`, Average
- `Sales[SalePrice]`, Sum
- `Sales[SaleID]`, Count
- `CustomerFeedback[Rating]`, Average
- `CustomerFeedback[LikelihoodToRecommend]`, Average

Sort by Sum of `Sales[SalePrice]` descending.

### Optional Visual: Account Age Group

If you created the optional `Account Age Group` column, use it in a column chart:

- X-axis: `Customers[Account Age Group]`
- Y-axis: `Sales[SalePrice]`, Sum
- Tooltips:
  - `Customers[CustomerID]`, Distinct count
  - `Sales[SaleID]`, Count

### Insights to Capture

- Highest revenue industry.
- Customer size group with strongest sales.
- Whether corporate or individual customers dominate transactions.
- Whether larger fleet customers create more revenue.
- Top customers by revenue and their feedback scores.

## Page 6: Service Analysis

### Purpose

Understand service workload, cost, downtime, replacement parts, and service quality.

### Slicers

- `DateTable[Year]`
- `Service[ServiceType]`
- `Service[PartReplaced]`
- `Dealers[Region]`
- `Trucks[TruckType]`
- `Trucks[Plant]`

### Visual 1: Service Events by Type

Visual type: Column chart

- X-axis: `Service[ServiceType]`
- Y-axis: `Service[ServiceID]`, Count
- Tooltips:
  - `Service[ServiceCost]`, Average
  - `Service[DowntimeDays]`, Average
  - `Service[FeedbackScore]`, Average

### Visual 2: Service Cost by Part Replaced

Visual type: Bar chart

- Y-axis: `Service[PartReplaced]`
- X-axis: `Service[ServiceCost]`, Sum
- Tooltips:
  - `Service[ServiceID]`, Count
  - `Service[ServiceCost]`, Average
  - `Service[DowntimeDays]`, Average

### Visual 3: Downtime Distribution

Visual type: Column chart

- X-axis: `Service[DowntimeDays]`
- Y-axis: `Service[ServiceID]`, Count
- Legend: `Service[ServiceType]`

### Visual 4: Service Cost Trend

Visual type: Line chart

- X-axis: `DateTable[Date]`
- Y-axis: `Service[ServiceCost]`, Sum
- Legend: `Service[ServiceType]`

### Visual 5: Downtime vs Feedback Score

Visual type: Scatter chart

- X-axis: `Service[DowntimeDays]`, Average
- Y-axis: `Service[FeedbackScore]`, Average
- Size: `Service[ServiceID]`, Count
- Legend: `Service[PartReplaced]`
- Details: `Dealers[DealerName]`

### Visual 6: Service Quality Matrix

Visual type: Matrix

- Rows: `Trucks[TruckType]`
- Columns: `Service[ServiceType]`
- Values:
  - `Service[ServiceID]`, Count
  - `Service[ServiceCost]`, Average
  - `Service[DowntimeDays]`, Average
  - `Service[FeedbackScore]`, Average

Use conditional formatting:

- Highlight high average downtime in red.
- Highlight low average service score in red.
- Highlight high average service score in green.

### Insights to Capture

- Most common service type.
- Most expensive replaced part.
- Parts linked with higher downtime.
- Dealers with lower feedback scores.
- Truck types with frequent or costly service records.

## Page 7: Feedback Analysis

### Purpose

Analyze customer ratings, delivery satisfaction, support satisfaction, recommendation score, and repeated feedback comments.

### Slicers

- `DateTable[Year]`
- `Trucks[TruckType]`
- `Trucks[Plant]`
- `Customers[IndustryType]`
- `Customers[Region]`
- `CustomerFeedback[Rating]`

### Visual 1: Average Rating by Truck Type

Visual type: Bar chart

- Y-axis: `Trucks[TruckType]`
- X-axis: `CustomerFeedback[Rating]`, Average
- Tooltips:
  - `CustomerFeedback[FeedbackID]`, Count
  - `CustomerFeedback[DeliverySatisfaction]`, Average
  - `CustomerFeedback[SupportSatisfaction]`, Average
  - `CustomerFeedback[LikelihoodToRecommend]`, Average

### Visual 2: Satisfaction by Plant and Truck Type

Visual type: Matrix

- Rows: `Trucks[Plant]`
- Columns: `Trucks[TruckType]`
- Values:
  - `CustomerFeedback[Rating]`, Average
  - `CustomerFeedback[DeliverySatisfaction]`, Average
  - `CustomerFeedback[SupportSatisfaction]`, Average
  - `CustomerFeedback[LikelihoodToRecommend]`, Average

Apply conditional formatting to identify lower scores.

### Visual 3: Recommendation Score Distribution

Visual type: Column chart

- X-axis: `CustomerFeedback[LikelihoodToRecommend]`
- Y-axis: `CustomerFeedback[FeedbackID]`, Count
- Legend: `Trucks[TruckType]`

### Visual 4: Rating by Customer Industry

Visual type: Bar chart

- Y-axis: `Customers[IndustryType]`
- X-axis: `CustomerFeedback[Rating]`, Average
- Tooltips:
  - `CustomerFeedback[FeedbackID]`, Count
  - `CustomerFeedback[LikelihoodToRecommend]`, Average

### Visual 5: Comment Summary Table

Visual type: Table

Columns:

- `CustomerFeedback[Comments]`
- `CustomerFeedback[FeedbackID]`, Count
- `CustomerFeedback[Rating]`, Average
- `CustomerFeedback[DeliverySatisfaction]`, Average
- `CustomerFeedback[SupportSatisfaction]`, Average
- `CustomerFeedback[LikelihoodToRecommend]`, Average

This is not sentiment analysis. Treat each repeated comment as a category and compare its average scores.

### Insights to Capture

- Best-rated truck type.
- Plant and truck type combinations with weaker feedback.
- Comments associated with low rating.
- Industries or regions with higher recommendation score.
- Whether delivery or support satisfaction appears weaker.

## Page 8: Truck Model Detail

### Purpose

Create a simple drill-through page for one selected truck model.

### Setup

1. Create a page named `Truck Model Detail`.
2. Add `Trucks[ModelName]` to the Drill-through field well.
3. Turn on `Keep all filters`.
4. Add a Back button.

### Visual 1: Model Profile

Visual type: Multi-row card

Fields:

- `Trucks[ModelName]`
- `Trucks[TruckType]`
- `Trucks[EngineType]`
- `Trucks[FuelType]`
- `Trucks[Plant]`
- `Trucks[BasePrice]`
- `Trucks[CapacityTons]`
- `Trucks[WarrantyMonths]`

### Visual 2: Model Summary Cards

Cards:

- `Sales[SalePrice]`, Sum
- `Sales[SaleID]`, Count
- `Production[QuantityProduced]`, Sum
- `Service[ServiceID]`, Count
- `Service[ServiceCost]`, Average
- `Service[DowntimeDays]`, Average
- `CustomerFeedback[Rating]`, Average
- `CustomerFeedback[LikelihoodToRecommend]`, Average

### Visual 3: Model Sales Trend

Visual type: Line chart

- X-axis: `DateTable[Date]`
- Y-axis: `Sales[SalePrice]`, Sum

### Visual 4: Model Service Breakdown

Visual type: Bar chart

- Y-axis: `Service[ServiceType]`
- X-axis: `Service[ServiceID]`, Count
- Tooltips:
  - `Service[ServiceCost]`, Average
  - `Service[DowntimeDays]`, Average

### Visual 5: Model Feedback Table

Visual type: Table

Columns:

- `CustomerFeedback[Comments]`
- `CustomerFeedback[FeedbackID]`, Count
- `CustomerFeedback[Rating]`, Average
- `CustomerFeedback[LikelihoodToRecommend]`, Average

### Insights to Capture

- Does the model sell well?
- Does it have many service events?
- Are service costs or downtime high?
- Are customer ratings strong?
- Which comments appear most often for the model?

## Page 9: Detail Tables

### Purpose

Provide record-level tables for simple investigation.

### Sales Detail Table

Columns:

- `Sales[SaleID]`
- `Sales[SaleDate]`
- `Trucks[ModelName]`
- `Trucks[TruckType]`
- `Customers[CustomerName]`
- `Dealers[DealerName]`
- `Sales[Region]`
- `Sales[SalePrice]`
- `Sales[PaymentType]`
- `Sales[SalesChannel]`

### Service Detail Table

Columns:

- `Service[ServiceID]`
- `Service[ServiceDate]`
- `Trucks[ModelName]`
- `Dealers[DealerName]`
- `Service[ServiceType]`
- `Service[PartReplaced]`
- `Service[ServiceCost]`
- `Service[FeedbackScore]`
- `Service[DowntimeDays]`

### Feedback Detail Table

Columns:

- `CustomerFeedback[FeedbackID]`
- `CustomerFeedback[FeedbackDate]`
- `Customers[CustomerName]`
- `Trucks[ModelName]`
- `CustomerFeedback[Rating]`
- `CustomerFeedback[DeliverySatisfaction]`
- `CustomerFeedback[SupportSatisfaction]`
- `CustomerFeedback[LikelihoodToRecommend]`
- `CustomerFeedback[Comments]`

## Slicers and Filters

Use slicers to let users explore the report without complex logic.

Recommended slicers by theme:

| Theme | Useful Slicers |
|---|---|
| Time | `DateTable[Year]`, `DateTable[Quarter]`, `DateTable[MonthName]` |
| Geography | `Sales[Region]`, `Dealers[Region]`, `Customers[Region]` |
| Trucks | `Trucks[TruckType]`, `Trucks[Plant]`, `Trucks[FuelType]`, `Trucks[EngineType]` |
| Customers | `Customers[IndustryType]`, `Customers[CompanySize]`, `Customers[CustomerType]` |
| Dealers | `Dealers[DealerType]`, `Dealers[DealerName]` |
| Service | `Service[ServiceType]`, `Service[PartReplaced]`, `Service[DowntimeDays]` |
| Feedback | `CustomerFeedback[Rating]`, `CustomerFeedback[LikelihoodToRecommend]` |

Keep slicers simple. Do not overcrowd every page. Use 4 to 6 slicers per page at most.

## Drill-down Instructions

Use drill-down mainly on date visuals.

Recommended date drill path:

1. `DateTable[Year]`
2. `DateTable[Quarter]`
3. `DateTable[MonthName]`
4. `DateTable[Date]`

Apply drill-down to:

- Sales revenue trend
- Production quantity trend
- Service cost trend
- Feedback rating trend, if created

How to use drill-down:

1. Start at the yearly view.
2. Select the drill-down icon on the visual.
3. Click a year to inspect quarters.
4. Click a quarter to inspect months.
5. Use slicers to compare one region, plant, dealer, or truck type at a time.

## Drill-through Instructions

Drill-through is optional but useful. Keep it simple.

Recommended drill-through pages:

| Source Field | Target Page | Purpose |
|---|---|---|
| `Trucks[ModelName]` | `Truck Model Detail` | Inspect one truck model |
| `Dealers[DealerName]` | `Detail Tables` | Inspect dealer sales and service records |
| `Customers[CustomerName]` | `Detail Tables` | Inspect customer sales and feedback records |
| `Trucks[Plant]` | `Detail Tables` | Inspect plant-related truck activity |

Always add a Back button to drill-through pages.

## Visual Interaction Guidance

Use default interactions first. Adjust only when needed.

Recommended behavior:

- Slicers should filter all visuals on the page.
- Clicking a bar in a chart should filter or highlight related visuals.
- Large detail tables can remain filtered by slicers and chart selections.
- If a visual becomes confusing when cross-highlighted, change its interaction to Filter or None.

Steps:

1. Select a visual.
2. Go to `Format` > `Edit interactions`.
3. Choose Filter, Highlight, or None for each other visual.
4. Test the page by selecting categories and clearing selections.

## Optional Tooltip Pages

Tooltip pages are optional. Use them only if the report needs extra hover detail.

### Truck Tooltip

Fields:

- `Trucks[ModelName]`
- `Trucks[TruckType]`
- `Trucks[Plant]`
- `Trucks[BasePrice]`
- Sum of `Sales[SalePrice]`
- Average of `CustomerFeedback[Rating]`
- Average of `Service[ServiceCost]`

### Dealer Tooltip

Fields:

- `Dealers[DealerName]`
- `Dealers[Region]`
- `Dealers[DealerType]`
- Sum of `Sales[SalePrice]`
- Count of `Sales[SaleID]`
- Average of `Service[FeedbackScore]`
- Average of `Service[DowntimeDays]`

## Formatting Standards

Use consistent formatting:

- Format `SalePrice`, `BasePrice`, `ProductionCost`, and `ServiceCost` as currency.
- Format average scores with one or two decimal places.
- Use display units such as Thousands or Millions for large currency charts.
- Sort ranked bar charts descending by the main value.
- Use conditional formatting in matrices for high and low values.
- Keep visual titles clear and business-friendly.
- Use consistent colors:
  - Sales: blue
  - Production: green
  - Service: orange or red
  - Feedback: teal or purple
- Avoid too many colors in one visual.
- Keep page layouts consistent.

## Suggested Analysis Flow

Use this flow when presenting the report:

1. Start on `Executive Overview`.
2. Select one year using the Year slicer.
3. Identify the strongest truck type and region.
4. Move to `Sales Analysis` to inspect channel, payment type, and top models.
5. Move to `Production Analysis` to compare production with sales demand.
6. Move to `Dealer and Region Analysis` to identify strong and weak dealers.
7. Move to `Customer Analysis` to understand who is buying.
8. Move to `Service Analysis` to inspect service cost and downtime.
9. Move to `Feedback Analysis` to compare ratings and recommendation scores.
10. Use drill-through or detail tables only when you need record-level investigation.

## Final Insights to Produce

Prepare a final written summary with these findings:

1. Top 3 truck types by sales revenue.
2. Top 10 truck models by sales revenue.
3. Best and weakest regions by sales revenue.
4. Most common sales channel.
5. Most common payment type.
6. Plant with highest production quantity.
7. Shift with highest production activity.
8. Machine line with highest production quantity.
9. Customer industry with highest sales revenue.
10. Customer size group with highest sales revenue.
11. Dealer with highest sales revenue.
12. Dealer with lowest average service feedback score.
13. Service type with most events.
14. Part replaced with highest total service cost.
15. Truck type with highest average customer rating.
16. Repeated feedback comment with lowest average rating.
17. Truck model that deserves further review because sales are high but rating or service score is weak.
18. Recommended business actions based only on charts, slicers, filters, and drill-through.

## Example Business Recommendations

Use the visuals to support practical recommendations such as:

- Increase focus on truck types with high revenue and strong ratings.
- Review production lines or shifts with high cost or low output.
- Support dealers that sell well but have low service feedback scores.
- Investigate parts that cause high service cost or downtime.
- Improve delivery or support processes if satisfaction scores are weak.
- Target customer industries that show high revenue and strong recommendation scores.
- Review truck models that sell well but receive lower ratings.

## Completion Checklist

Before submitting the report, confirm that:

- All 8 CSV files are loaded.
- Data types are correct.
- Relationships are active and one-to-many where expected.
- `DateTable` is marked as the date table.
- Report pages use visual aggregations instead of complex DAX measures.
- Optional calculated columns, if used, are simple and easy to explain.
- Slicers work correctly.
- Drill-down works on date visuals.
- Drill-through pages work if included.
- Tables and matrices use clear sorting and conditional formatting.
- Each page answers at least one business question.
- Final insights are based only on visualization, slicing, filtering, drill-down, and drill-through.

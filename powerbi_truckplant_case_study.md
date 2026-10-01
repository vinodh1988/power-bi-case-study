# Power BI Case Study: Truck Plant Dataset Analysis

## Goal

Build a 5-page Power BI report for the truck plant dataset. The report must use only native Power BI visuals, slicers, filters, drill-down, drill-through, and simple visual aggregations.

Do not use Python, R, Copilot, Q&A, Smart Narrative, Key Influencers, AI Insights, text analytics, machine learning, or external scripts.

Avoid complex DAX measures. Use the built-in aggregation options inside visuals: `Sum`, `Average`, `Count`, `Distinct count`, `Minimum`, and `Maximum`.

The final report must contain exactly these 5 pages:

1. `Executive Overview`
2. `Sales and Customers`
3. `Production Operations`
4. `Service and Feedback`
5. `Dealer and Truck Detail`

## Dataset Files

Load these 8 CSV files into Power BI Desktop:

| File | Main Use |
|---|---|
| `Customers.csv` | Customer profile, region, industry, company size, fleet size, customer type |
| `Trucks.csv` | Truck model, truck type, engine, fuel, plant, capacity, warranty, base price |
| `Dealers.csv` | Dealer name, region, dealer type, staff count, opening date |
| `DateTable.csv` | Calendar fields for year, quarter, month, week, and day |
| `Production.csv` | Production date, plant, quantity, cost, shift, machine line, supervisor |
| `Sales.csv` | Sale date, sale price, customer, truck, dealer, region, payment, channel |
| `Service.csv` | Service date, service type, cost, dealer, feedback score, downtime, part replaced |
| `CustomerFeedback.csv` | Rating, delivery satisfaction, support satisfaction, recommendation, comments |

## Part 1: Import the CSV Files

Follow these exact steps:

1. Open Power BI Desktop.
2. Select `Home` > `Get Data` > `Text/CSV`.
3. Choose `Customers.csv`.
4. Confirm the preview shows column headers correctly.
5. Select `Load` or `Transform Data`.
6. Repeat the same process for all remaining CSV files:
   - `Trucks.csv`
   - `Dealers.csv`
   - `DateTable.csv`
   - `Production.csv`
   - `Sales.csv`
   - `Service.csv`
   - `CustomerFeedback.csv`
7. After all files are selected, open `Transform Data` if Power Query is not already open.

## Part 2: Set Data Types in Power Query

In Power Query, select each table and set data types exactly as below.

### Customers

| Column | Type |
|---|---|
| `CustomerID` | Whole Number |
| `CustomerName` | Text |
| `IndustryType` | Text |
| `Region` | Text |
| `CompanySize` | Text |
| `AccountSince` | Date |
| `FleetSize` | Whole Number |
| `CustomerType` | Text |
| `Contact` | Text |

### Trucks

| Column | Type |
|---|---|
| `TruckID` | Whole Number |
| `ModelName` | Text |
| `TruckType` | Text |
| `EngineType` | Text |
| `FuelType` | Text |
| `LaunchDate` | Date |
| `BasePrice` | Decimal Number |
| `Color` | Text |
| `CapacityTons` | Whole Number |
| `Plant` | Text |
| `WarrantyMonths` | Whole Number |

### Dealers

| Column | Type |
|---|---|
| `DealerID` | Whole Number |
| `DealerName` | Text |
| `Region` | Text |
| `OpeningDate` | Date |
| `TotalSales` | Whole Number |
| `DealerType` | Text |
| `StaffCount` | Whole Number |

### DateTable

| Column | Type |
|---|---|
| `Date` | Date |
| `Year` | Whole Number |
| `Month` | Whole Number |
| `Day` | Whole Number |
| `DayOfWeek` | Whole Number |
| `DayName` | Text |
| `MonthName` | Text |
| `Quarter` | Text |
| `WeekOfYear` | Whole Number |

### Production

| Column | Type |
|---|---|
| `ProductionID` | Whole Number |
| `TruckID` | Whole Number |
| `Plant` | Text |
| `ProductionDate` | Date |
| `QuantityProduced` | Whole Number |
| `ProductionCost` | Decimal Number |
| `Shift` | Text |
| `Supervisor` | Text |
| `MachineUsed` | Text |

### Sales

| Column | Type |
|---|---|
| `SaleID` | Whole Number |
| `TruckID` | Whole Number |
| `CustomerID` | Whole Number |
| `SaleDate` | Date |
| `SalePrice` | Decimal Number |
| `DealerID` | Whole Number |
| `Region` | Text |
| `PaymentType` | Text |
| `Financing` | Text |
| `SalesChannel` | Text |

### Service

| Column | Type |
|---|---|
| `ServiceID` | Whole Number |
| `TruckID` | Whole Number |
| `ServiceDate` | Date |
| `ServiceType` | Text |
| `ServiceCost` | Decimal Number |
| `DealerID` | Whole Number |
| `FeedbackScore` | Whole Number |
| `DowntimeDays` | Whole Number |
| `PartReplaced` | Text |

### CustomerFeedback

| Column | Type |
|---|---|
| `FeedbackID` | Whole Number |
| `CustomerID` | Whole Number |
| `TruckID` | Whole Number |
| `FeedbackDate` | Date |
| `Rating` | Whole Number |
| `Comments` | Text |
| `DeliverySatisfaction` | Whole Number |
| `SupportSatisfaction` | Whole Number |
| `LikelihoodToRecommend` | Whole Number |

After setting data types:

1. Select `Close & Apply`.
2. Wait for Power BI to load all tables.
3. Save the file as `TruckPlant_CaseStudy.pbix`.

## Part 3: Create Relationships

Open `Model view` and create the relationships below. Use `Single` cross-filter direction for all relationships.

| From Table | From Column | To Table | To Column | Cardinality |
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

Then mark the date table:

1. Select `DateTable`.
2. Go to `Table tools`.
3. Select `Mark as date table`.
4. Choose `DateTable[Date]`.
5. Confirm.

## Part 4: Use Simple Aggregations Instead of Measures

Do not create a measure table. For each visual, drag the field into the visual and set the summarization.

Use this guide:

| Metric Needed | Field to Drag | Summarization |
|---|---|---|
| Sales revenue | `Sales[SalePrice]` | Sum |
| Number of sales | `Sales[SaleID]` | Count |
| Average sale price | `Sales[SalePrice]` | Average |
| Production quantity | `Production[QuantityProduced]` | Sum |
| Production cost | `Production[ProductionCost]` | Sum |
| Average production cost | `Production[ProductionCost]` | Average |
| Number of production records | `Production[ProductionID]` | Count |
| Service cost | `Service[ServiceCost]` | Sum |
| Average service cost | `Service[ServiceCost]` | Average |
| Number of service events | `Service[ServiceID]` | Count |
| Average service score | `Service[FeedbackScore]` | Average |
| Average downtime | `Service[DowntimeDays]` | Average |
| Feedback responses | `CustomerFeedback[FeedbackID]` | Count |
| Average rating | `CustomerFeedback[Rating]` | Average |
| Average delivery satisfaction | `CustomerFeedback[DeliverySatisfaction]` | Average |
| Average support satisfaction | `CustomerFeedback[SupportSatisfaction]` | Average |
| Average recommendation score | `CustomerFeedback[LikelihoodToRecommend]` | Average |
| Customer count | `Customers[CustomerID]` | Distinct count |
| Truck model count | `Trucks[TruckID]` | Distinct count |

## Optional Simple Calculated Columns

These columns are optional. Add them only if you want easier grouping in visuals.

### Optional Column 1: Price Band

Create in `Trucks`:

```DAX
Price Band =
SWITCH(
    TRUE(),
    Trucks[BasePrice] < 75000, "Low Price",
    Trucks[BasePrice] < 120000, "Mid Price",
    "High Price"
)
```

Use this in slicers or bar charts.

### Optional Column 2: Downtime Group

Create in `Service`:

```DAX
Downtime Group =
SWITCH(
    TRUE(),
    Service[DowntimeDays] <= 1, "Low Downtime",
    Service[DowntimeDays] <= 3, "Medium Downtime",
    "High Downtime"
)
```

Use this for service quality visuals.

If you want the simplest report, skip both calculated columns.

# Report Page 1: Executive Overview

## Purpose

This page gives the management team a one-screen summary of sales, production, service, and customer satisfaction.

## Page Setup

1. Rename Page 1 to `Executive Overview`.
2. Open `Format page`.
3. Set canvas size to `16:9`.
4. Add a page title text box: `Executive Overview`.
5. Place slicers in a horizontal row below the title.
6. Place KPI cards under the slicers.
7. Place charts below the KPI cards.

## Slicers

Create these slicers:

### Slicer 1: Year

1. Add a slicer visual.
2. Drag `DateTable[Year]` into Field.
3. Set style to Dropdown.
4. Turn on Single select only if you want one year at a time.

### Slicer 2: Quarter

1. Add a slicer visual.
2. Drag `DateTable[Quarter]` into Field.
3. Set style to Dropdown.

### Slicer 3: Truck Type

1. Add a slicer visual.
2. Drag `Trucks[TruckType]` into Field.
3. Set style to Tile or Dropdown.

### Slicer 4: Plant

1. Add a slicer visual.
2. Drag `Trucks[Plant]` into Field.
3. Set style to Dropdown.

### Slicer 5: Region

1. Add a slicer visual.
2. Drag `Sales[Region]` into Field.
3. Set style to Dropdown.

## KPI Cards

Create 8 card visuals.

### Card 1: Sales Revenue

1. Insert a Card visual.
2. Drag `Sales[SalePrice]` into Data.
3. In the field dropdown, choose `Sum`.
4. Rename the visual title to `Total Sales Revenue`.
5. Format as currency.

### Card 2: Sales Count

1. Insert a Card visual.
2. Drag `Sales[SaleID]` into Data.
3. Choose `Count`.
4. Title: `Sales Transactions`.

### Card 3: Production Quantity

1. Insert a Card visual.
2. Drag `Production[QuantityProduced]` into Data.
3. Choose `Sum`.
4. Title: `Quantity Produced`.

### Card 4: Production Cost

1. Insert a Card visual.
2. Drag `Production[ProductionCost]` into Data.
3. Choose `Sum`.
4. Title: `Production Cost`.
5. Format as currency.

### Card 5: Service Events

1. Insert a Card visual.
2. Drag `Service[ServiceID]` into Data.
3. Choose `Count`.
4. Title: `Service Events`.

### Card 6: Average Rating

1. Insert a Card visual.
2. Drag `CustomerFeedback[Rating]` into Data.
3. Choose `Average`.
4. Title: `Average Customer Rating`.
5. Set decimal places to 1 or 2.

### Card 7: Average Service Score

1. Insert a Card visual.
2. Drag `Service[FeedbackScore]` into Data.
3. Choose `Average`.
4. Title: `Average Service Score`.

### Card 8: Average Recommendation

1. Insert a Card visual.
2. Drag `CustomerFeedback[LikelihoodToRecommend]` into Data.
3. Choose `Average`.
4. Title: `Average Recommendation Score`.

## Visual 1: Sales Revenue Trend

Visual type: Line chart

Steps:

1. Insert a Line chart.
2. Drag `DateTable[Date]` to X-axis.
3. Drag `Sales[SalePrice]` to Y-axis.
4. Set `Sales[SalePrice]` summarization to `Sum`.
5. Drag `DateTable[Year]` to Legend.
6. Turn on data labels if the chart is readable.
7. Title the visual `Sales Revenue Trend`.
8. Use the date drill controls to move between Year, Quarter, Month, and Day.

What to observe:

- Years with increasing or declining sales revenue.
- Quarters with unusually high or low revenue.
- Whether selected truck types or regions change the trend.

## Visual 2: Revenue by Truck Type

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Trucks[TruckType]` to Y-axis.
3. Drag `Sales[SalePrice]` to X-axis.
4. Set `Sales[SalePrice]` to `Sum`.
5. Add tooltips:
   - `Sales[SaleID]`, Count
   - `Sales[SalePrice]`, Average
   - `CustomerFeedback[Rating]`, Average
6. Sort descending by Sum of `SalePrice`.
7. Title the visual `Revenue by Truck Type`.

What to observe:

- Which truck type produces the most revenue.
- Whether the leader changes after filtering by year, region, or plant.

## Visual 3: Production by Plant

Visual type: Clustered column chart

Steps:

1. Insert a Clustered column chart.
2. Drag `Trucks[Plant]` or `Production[Plant]` to X-axis.
3. Drag `Production[QuantityProduced]` to Y-axis.
4. Set `QuantityProduced` to `Sum`.
5. Add tooltips:
   - `Production[ProductionCost]`, Sum
   - `Production[ProductionCost]`, Average
   - `Production[ProductionID]`, Count
6. Title the visual `Production Quantity by Plant`.

What to observe:

- Plant with highest total production.
- Whether a high-production plant also has high production cost.

## Visual 4: Service Events by Type

Visual type: Clustered column chart

Steps:

1. Insert a Clustered column chart.
2. Drag `Service[ServiceType]` to X-axis.
3. Drag `Service[ServiceID]` to Y-axis.
4. Set `ServiceID` to `Count`.
5. Add tooltips:
   - `Service[ServiceCost]`, Average
   - `Service[DowntimeDays]`, Average
   - `Service[FeedbackScore]`, Average
6. Title the visual `Service Events by Type`.

What to observe:

- Most common service type.
- Whether one type has higher average cost or downtime.

## Visual 5: Average Feedback by Plant and Truck Type

Visual type: Matrix

Steps:

1. Insert a Matrix visual.
2. Drag `Trucks[Plant]` to Rows.
3. Drag `Trucks[TruckType]` to Columns.
4. Drag `CustomerFeedback[Rating]` to Values.
5. Set summarization to `Average`.
6. Drag `CustomerFeedback[LikelihoodToRecommend]` to Values.
7. Set summarization to `Average`.
8. Apply conditional formatting:
   - Select the dropdown beside Average of Rating.
   - Choose Conditional formatting > Background color.
   - Use lower values as red and higher values as green.
9. Title the visual `Feedback by Plant and Truck Type`.

What to observe:

- Plant and truck type combinations with weak feedback.
- Combinations with strong rating and recommendation.

## Page 1 Required Insights

Write short notes for these items:

1. Highest revenue truck type.
2. Highest production plant.
3. Most common service type.
4. Overall average customer rating.
5. Any major mismatch between strong sales and weak rating.

# Report Page 2: Sales and Customers

## Purpose

This page explains who is buying trucks, what they are buying, where sales happen, and which customer segments produce the most revenue.

## Page Setup

1. Create a new page.
2. Rename it `Sales and Customers`.
3. Place slicers on the left side.
4. Place sales visuals on the top half.
5. Place customer visuals on the bottom half.

## Slicers

Create these slicers:

- `DateTable[Year]`
- `Sales[Region]`
- `Trucks[TruckType]`
- `Sales[SalesChannel]`
- `Sales[PaymentType]`
- `Customers[IndustryType]`
- `Customers[CompanySize]`

Use dropdown style for slicers with more than 3 values.

## Visual 1: Revenue by Region

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Sales[Region]` to Y-axis.
3. Drag `Sales[SalePrice]` to X-axis.
4. Set `SalePrice` to `Sum`.
5. Add tooltips:
   - `Sales[SaleID]`, Count
   - `Sales[SalePrice]`, Average
   - `Customers[CustomerID]`, Distinct count
6. Sort descending by Sum of `SalePrice`.
7. Title: `Sales Revenue by Region`.

Insight:

- Identify the strongest and weakest sales regions.

## Visual 2: Monthly Revenue by Region

Visual type: Line chart

Steps:

1. Insert a Line chart.
2. Drag `DateTable[Date]` to X-axis.
3. Drag `Sales[SalePrice]` to Y-axis.
4. Set `SalePrice` to `Sum`.
5. Drag `Sales[Region]` to Legend.
6. Title: `Monthly Revenue by Region`.
7. Use drill-down to inspect Year > Quarter > Month.

Insight:

- Look for region trends and seasonal movement.

## Visual 3: Sales Channel and Truck Type Matrix

Visual type: Matrix

Steps:

1. Insert a Matrix visual.
2. Drag `Trucks[TruckType]` to Rows.
3. Drag `Sales[SalesChannel]` to Columns.
4. Drag `Sales[SalePrice]` to Values and set to `Sum`.
5. Drag `Sales[SaleID]` to Values and set to `Count`.
6. Drag `Sales[SalePrice]` to Values a second time and set to `Average`.
7. Rename the value labels in the visual pane if needed:
   - Sum of SalePrice = Revenue
   - Count of SaleID = Sales Count
   - Average of SalePrice = Avg Sale Price
8. Apply conditional formatting to Revenue.
9. Title: `Truck Type by Sales Channel`.

Insight:

- Compare dealer and online sales by truck type.

## Visual 4: Payment Type Mix

Visual type: 100% stacked column chart

Steps:

1. Insert a 100% stacked column chart.
2. Drag `Sales[Region]` to X-axis.
3. Drag `Sales[PaymentType]` to Legend.
4. Drag `Sales[SaleID]` to Y-axis and set to `Count`.
5. Title: `Payment Mix by Region`.

Insight:

- Identify regions where financing is more common.

## Visual 5: Top 10 Truck Models by Revenue

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Trucks[ModelName]` to Y-axis.
3. Drag `Sales[SalePrice]` to X-axis.
4. Set `SalePrice` to `Sum`.
5. Add tooltips:
   - `Trucks[TruckType]`
   - `Trucks[BasePrice]`, Average
   - `Trucks[CapacityTons]`, Average
   - `Sales[SaleID]`, Count
   - `CustomerFeedback[Rating]`, Average
6. In Filters for this visual, add `Trucks[ModelName]`.
7. Change filter type to `Top N`.
8. Show items: Top `10`.
9. By value: drag `Sales[SalePrice]` and set to Sum.
10. Select `Apply filter`.
11. Sort descending by Sum of `SalePrice`.
12. Title: `Top 10 Truck Models by Revenue`.

Insight:

- Identify truck models that drive the most sales.

## Visual 6: Revenue by Customer Industry

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Customers[IndustryType]` to Y-axis.
3. Drag `Sales[SalePrice]` to X-axis.
4. Set `SalePrice` to `Sum`.
5. Add tooltips:
   - `Customers[CustomerID]`, Distinct count
   - `Sales[SaleID]`, Count
   - `CustomerFeedback[LikelihoodToRecommend]`, Average
6. Sort descending by Sum of `SalePrice`.
7. Title: `Revenue by Customer Industry`.

Insight:

- Identify the customer industry that contributes the most revenue.

## Visual 7: Fleet Size vs Revenue

Visual type: Scatter chart

Steps:

1. Insert a Scatter chart.
2. Drag `Customers[FleetSize]` to X-axis and set to `Average`.
3. Drag `Sales[SalePrice]` to Y-axis and set to `Sum`.
4. Drag `Sales[SaleID]` to Size and set to `Count`.
5. Drag `Customers[CompanySize]` to Legend.
6. Drag `Customers[CustomerName]` to Details.
7. Add tooltip `Customers[IndustryType]`.
8. Add tooltip `CustomerFeedback[Rating]`, Average.
9. Title: `Fleet Size vs Revenue`.

Insight:

- Check whether larger fleets are associated with higher revenue.

## Page 2 Required Insights

Write short notes for these items:

1. Best sales region.
2. Best customer industry.
3. Most common payment type by region.
4. Strongest sales channel by truck type.
5. Top truck model by revenue.
6. Whether larger fleets appear to buy more.

# Report Page 3: Production Operations

## Purpose

This page explains how much the plants produce, which shifts and machine lines are active, and where production cost is higher.

## Page Setup

1. Create a new page.
2. Rename it `Production Operations`.
3. Place slicers at the top.
4. Place plant-level visuals in the first row.
5. Place shift and machine-line visuals in the second row.
6. Place trend and scatter visuals in the lower section.

## Slicers

Create these slicers:

- `DateTable[Year]`
- `DateTable[Quarter]`
- `Production[Plant]`
- `Production[Shift]`
- `Production[MachineUsed]`
- `Trucks[TruckType]`

## Visual 1: Quantity Produced by Plant

Visual type: Clustered column chart

Steps:

1. Insert a Clustered column chart.
2. Drag `Production[Plant]` to X-axis.
3. Drag `Production[QuantityProduced]` to Y-axis.
4. Set `QuantityProduced` to `Sum`.
5. Add tooltips:
   - `Production[ProductionID]`, Count
   - `Production[ProductionCost]`, Sum
   - `Production[ProductionCost]`, Average
6. Sort descending by Sum of `QuantityProduced`.
7. Title: `Quantity Produced by Plant`.

Insight:

- Identify the plant with the most output.

## Visual 2: Production Cost by Plant

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Production[Plant]` to Y-axis.
3. Drag `Production[ProductionCost]` to X-axis.
4. Set `ProductionCost` to `Sum`.
5. Add tooltips:
   - `Production[QuantityProduced]`, Sum
   - `Production[ProductionCost]`, Average
6. Sort descending by Sum of `ProductionCost`.
7. Title: `Production Cost by Plant`.

Insight:

- Compare total cost with quantity produced.

## Visual 3: Plant and Shift Matrix

Visual type: Matrix

Steps:

1. Insert a Matrix visual.
2. Drag `Production[Plant]` to Rows.
3. Drag `Production[Shift]` to Columns.
4. Drag `Production[QuantityProduced]` to Values and set to `Sum`.
5. Drag `Production[ProductionCost]` to Values and set to `Sum`.
6. Drag `Production[ProductionCost]` to Values again and set to `Average`.
7. Apply conditional formatting to Average of `ProductionCost`:
   - Low values: green
   - High values: red
8. Title: `Plant and Shift Production Matrix`.

Insight:

- Identify high-output and high-cost shift combinations.

## Visual 4: Production by Machine Line

Visual type: Stacked bar chart

Steps:

1. Insert a Stacked bar chart.
2. Drag `Production[MachineUsed]` to Y-axis.
3. Drag `Production[QuantityProduced]` to X-axis.
4. Set `QuantityProduced` to `Sum`.
5. Drag `Production[Shift]` to Legend.
6. Add tooltip `Production[ProductionCost]`, Average.
7. Title: `Production by Machine Line and Shift`.

Insight:

- Identify the busiest production line and which shift uses it most.

## Visual 5: Production Quantity Trend

Visual type: Line chart

Steps:

1. Insert a Line chart.
2. Drag `DateTable[Date]` to X-axis.
3. Drag `Production[QuantityProduced]` to Y-axis.
4. Set `QuantityProduced` to `Sum`.
5. Drag `Production[Plant]` to Legend.
6. Turn on drill-down.
7. Title: `Production Quantity Trend by Plant`.

Insight:

- Look for production increases, dips, or plant-level changes over time.

## Visual 6: Truck Capacity vs Production Cost

Visual type: Scatter chart

Steps:

1. Insert a Scatter chart.
2. Drag `Trucks[CapacityTons]` to X-axis and set to `Average`.
3. Drag `Production[ProductionCost]` to Y-axis and set to `Average`.
4. Drag `Production[QuantityProduced]` to Size and set to `Sum`.
5. Drag `Trucks[TruckType]` to Legend.
6. Drag `Trucks[ModelName]` to Details.
7. Add tooltip `Trucks[Plant]`.
8. Add tooltip `Trucks[BasePrice]`, Average.
9. Title: `Capacity vs Average Production Cost`.

Insight:

- Check whether higher-capacity trucks tend to cost more to produce.

## Visual 7: Supervisor Production Table

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add `Production[Supervisor]`.
3. Add `Production[Plant]`.
4. Add `Production[Shift]`.
5. Add `Production[ProductionID]` and set to `Count`.
6. Add `Production[QuantityProduced]` and set to `Sum`.
7. Add `Production[ProductionCost]` and set to `Average`.
8. Sort by Sum of `QuantityProduced` descending.
9. Title: `Supervisor Production Summary`.

Insight:

- Identify supervisors associated with high output or high average cost.

## Page 3 Required Insights

Write short notes for these items:

1. Plant with highest production quantity.
2. Plant with highest total production cost.
3. Shift with highest production quantity.
4. Machine line with highest production quantity.
5. Truck type with highest average production cost.
6. Any visible production trend by year or quarter.

# Report Page 4: Service and Feedback

## Purpose

This page connects service activity with customer feedback. It should show service cost, downtime, service scores, customer ratings, recommendation scores, and repeated customer comments.

## Page Setup

1. Create a new page.
2. Rename it `Service and Feedback`.
3. Place service slicers at the top left.
4. Place feedback slicers at the top right.
5. Place service visuals in the upper half.
6. Place feedback visuals in the lower half.

## Slicers

Create these slicers:

- `DateTable[Year]`
- `Service[ServiceType]`
- `Service[PartReplaced]`
- `Trucks[TruckType]`
- `Trucks[Plant]`
- `Customers[IndustryType]`
- `CustomerFeedback[Rating]`

If you created optional `Service[Downtime Group]`, add it as a slicer too.

## Visual 1: Service Events by Type

Visual type: Clustered column chart

Steps:

1. Insert a Clustered column chart.
2. Drag `Service[ServiceType]` to X-axis.
3. Drag `Service[ServiceID]` to Y-axis.
4. Set `ServiceID` to `Count`.
5. Add tooltips:
   - `Service[ServiceCost]`, Average
   - `Service[DowntimeDays]`, Average
   - `Service[FeedbackScore]`, Average
6. Title: `Service Events by Type`.

Insight:

- Identify the most common service type.

## Visual 2: Service Cost by Part Replaced

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Service[PartReplaced]` to Y-axis.
3. Drag `Service[ServiceCost]` to X-axis.
4. Set `ServiceCost` to `Sum`.
5. Add tooltips:
   - `Service[ServiceID]`, Count
   - `Service[ServiceCost]`, Average
   - `Service[DowntimeDays]`, Average
6. Sort descending by Sum of `ServiceCost`.
7. Title: `Service Cost by Part Replaced`.

Insight:

- Identify the part that creates the highest service cost.

## Visual 3: Downtime Distribution

Visual type: Clustered column chart

Steps:

1. Insert a Clustered column chart.
2. Drag `Service[DowntimeDays]` to X-axis.
3. Drag `Service[ServiceID]` to Y-axis.
4. Set `ServiceID` to `Count`.
5. Drag `Service[ServiceType]` to Legend.
6. Title: `Downtime Distribution by Service Type`.

Insight:

- Identify whether downtime is usually low or high.

## Visual 4: Dealer Service Quality

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Dealers[DealerName]` to Y-axis.
3. Drag `Service[FeedbackScore]` to X-axis.
4. Set `FeedbackScore` to `Average`.
5. Add tooltips:
   - `Service[ServiceID]`, Count
   - `Service[DowntimeDays]`, Average
   - `Service[ServiceCost]`, Average
6. Add a visual-level Top N filter:
   - Filter `Dealers[DealerName]`.
   - Choose Top N.
   - Show Top 10 by Average of `Service[FeedbackScore]`.
7. Duplicate this visual.
8. Change the duplicate to Bottom 10 by Average of `Service[FeedbackScore]`.
9. Title the visuals `Top 10 Dealer Service Scores` and `Bottom 10 Dealer Service Scores`.

Insight:

- Identify dealers with strong and weak service quality.

## Visual 5: Average Rating by Truck Type

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Trucks[TruckType]` to Y-axis.
3. Drag `CustomerFeedback[Rating]` to X-axis.
4. Set `Rating` to `Average`.
5. Add tooltips:
   - `CustomerFeedback[FeedbackID]`, Count
   - `CustomerFeedback[DeliverySatisfaction]`, Average
   - `CustomerFeedback[SupportSatisfaction]`, Average
   - `CustomerFeedback[LikelihoodToRecommend]`, Average
6. Sort descending by Average of `Rating`.
7. Title: `Average Rating by Truck Type`.

Insight:

- Identify the truck type with the best customer rating.

## Visual 6: Satisfaction Matrix

Visual type: Matrix

Steps:

1. Insert a Matrix visual.
2. Drag `Trucks[Plant]` to Rows.
3. Drag `Trucks[TruckType]` to Columns.
4. Drag `CustomerFeedback[Rating]` to Values and set to `Average`.
5. Drag `CustomerFeedback[DeliverySatisfaction]` to Values and set to `Average`.
6. Drag `CustomerFeedback[SupportSatisfaction]` to Values and set to `Average`.
7. Drag `CustomerFeedback[LikelihoodToRecommend]` to Values and set to `Average`.
8. Apply conditional formatting to each score field:
   - Low scores: red
   - Medium scores: yellow
   - High scores: green
9. Title: `Satisfaction by Plant and Truck Type`.

Insight:

- Identify weak plant and truck type combinations.

## Visual 7: Comment Summary

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add `CustomerFeedback[Comments]`.
3. Add `CustomerFeedback[FeedbackID]` and set to `Count`.
4. Add `CustomerFeedback[Rating]` and set to `Average`.
5. Add `CustomerFeedback[DeliverySatisfaction]` and set to `Average`.
6. Add `CustomerFeedback[SupportSatisfaction]` and set to `Average`.
7. Add `CustomerFeedback[LikelihoodToRecommend]` and set to `Average`.
8. Sort by Average of `Rating` ascending to find weaker comments.
9. Title: `Feedback Comments Summary`.

Important:

- Do not use sentiment analysis.
- Do not use AI text features.
- Treat each repeated comment as a normal category.

## Page 4 Required Insights

Write short notes for these items:

1. Most common service type.
2. Part with highest total service cost.
3. Dealer with strongest service score.
4. Dealer with weakest service score.
5. Truck type with highest customer rating.
6. Plant and truck type combination with weakest satisfaction.
7. Comment category with lowest average rating.

# Report Page 5: Dealer and Truck Detail

## Purpose

This page is the investigation page. It should allow users to inspect one dealer, one truck model, one plant, or one customer group in more detail. It combines ranked summaries and record-level tables.

## Page Setup

1. Create a new page.
2. Rename it `Dealer and Truck Detail`.
3. Add a Back button:
   - Select `Insert` > `Buttons` > `Back`.
   - Place it in the top-left corner.
4. Add drill-through fields:
   - In the Drill-through pane, add `Dealers[DealerName]`.
   - Add `Trucks[ModelName]`.
   - Add `Trucks[Plant]`.
   - Add `Customers[IndustryType]`.
5. Turn on `Keep all filters`.
6. Place summary visuals at the top.
7. Place detail tables at the bottom.

## Slicers

Create these slicers:

- `DateTable[Year]`
- `Dealers[Region]`
- `Dealers[DealerType]`
- `Trucks[TruckType]`
- `Trucks[ModelName]`
- `Customers[IndustryType]`

## Visual 1: Dealer Leaderboard

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add `Dealers[DealerName]`.
3. Add `Dealers[Region]`.
4. Add `Dealers[DealerType]`.
5. Add `Dealers[StaffCount]` and set to `Average`.
6. Add `Sales[SalePrice]` and set to `Sum`.
7. Add `Sales[SaleID]` and set to `Count`.
8. Add `Sales[SalePrice]` again and set to `Average`.
9. Add `Service[ServiceID]` and set to `Count`.
10. Add `Service[FeedbackScore]` and set to `Average`.
11. Add `Service[DowntimeDays]` and set to `Average`.
12. Sort by Sum of `Sales[SalePrice]` descending.
13. Title: `Dealer Sales and Service Leaderboard`.

Insight:

- Find dealers with high revenue and strong service score.
- Find dealers with high revenue but weak service score.

## Visual 2: Revenue by Dealer Type

Visual type: Clustered bar chart

Steps:

1. Insert a Clustered bar chart.
2. Drag `Dealers[DealerType]` to Y-axis.
3. Drag `Sales[SalePrice]` to X-axis.
4. Set `SalePrice` to `Sum`.
5. Add tooltips:
   - `Sales[SaleID]`, Count
   - `Sales[SalePrice]`, Average
   - `Service[FeedbackScore]`, Average
6. Title: `Revenue by Dealer Type`.

Insight:

- Compare exclusive and multi-brand dealers.

## Visual 3: Truck Model Summary Table

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add `Trucks[ModelName]`.
3. Add `Trucks[TruckType]`.
4. Add `Trucks[Plant]`.
5. Add `Trucks[BasePrice]` and set to `Average`.
6. Add `Trucks[CapacityTons]` and set to `Average`.
7. Add `Sales[SalePrice]` and set to `Sum`.
8. Add `Sales[SaleID]` and set to `Count`.
9. Add `Production[QuantityProduced]` and set to `Sum`.
10. Add `Service[ServiceID]` and set to `Count`.
11. Add `Service[ServiceCost]` and set to `Average`.
12. Add `CustomerFeedback[Rating]` and set to `Average`.
13. Add `CustomerFeedback[LikelihoodToRecommend]` and set to `Average`.
14. Sort by Sum of `Sales[SalePrice]` descending.
15. Title: `Truck Model Performance Summary`.

Insight:

- Identify truck models with strong revenue and weak feedback.
- Identify truck models with high service activity.

## Visual 4: Sales Detail Table

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add these columns:
   - `Sales[SaleID]`
   - `Sales[SaleDate]`
   - `Trucks[ModelName]`
   - `Trucks[TruckType]`
   - `Customers[CustomerName]`
   - `Customers[IndustryType]`
   - `Dealers[DealerName]`
   - `Sales[Region]`
   - `Sales[SalePrice]`
   - `Sales[PaymentType]`
   - `Sales[SalesChannel]`
3. Sort by `Sales[SaleDate]` descending.
4. Title: `Sales Detail`.

Use:

- Inspect transactions after selecting a dealer, model, region, or customer industry.

## Visual 5: Service Detail Table

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add these columns:
   - `Service[ServiceID]`
   - `Service[ServiceDate]`
   - `Trucks[ModelName]`
   - `Trucks[TruckType]`
   - `Dealers[DealerName]`
   - `Service[ServiceType]`
   - `Service[PartReplaced]`
   - `Service[ServiceCost]`
   - `Service[FeedbackScore]`
   - `Service[DowntimeDays]`
3. Sort by `Service[ServiceDate]` descending.
4. Title: `Service Detail`.

Use:

- Inspect service records for the selected dealer or truck model.

## Visual 6: Feedback Detail Table

Visual type: Table

Steps:

1. Insert a Table visual.
2. Add these columns:
   - `CustomerFeedback[FeedbackID]`
   - `CustomerFeedback[FeedbackDate]`
   - `Customers[CustomerName]`
   - `Customers[IndustryType]`
   - `Trucks[ModelName]`
   - `Trucks[TruckType]`
   - `CustomerFeedback[Rating]`
   - `CustomerFeedback[DeliverySatisfaction]`
   - `CustomerFeedback[SupportSatisfaction]`
   - `CustomerFeedback[LikelihoodToRecommend]`
   - `CustomerFeedback[Comments]`
3. Sort by `CustomerFeedback[FeedbackDate]` descending.
4. Title: `Feedback Detail`.

Use:

- Inspect actual customer feedback rows after filtering by model, plant, truck type, or industry.

## Drill-through Setup from Other Pages

Create drill-through access into Page 5.

### From Page 2: Top Truck Models by Revenue

1. Go to `Sales and Customers`.
2. Right-click a truck model in `Top 10 Truck Models by Revenue`.
3. Select Drill through > `Dealer and Truck Detail`.
4. Confirm Page 5 opens filtered to that truck model.

### From Page 2: Revenue by Customer Industry

1. Right-click an industry in `Revenue by Customer Industry`.
2. Select Drill through > `Dealer and Truck Detail`.
3. Confirm Page 5 opens filtered to that industry.

### From Page 3: Quantity Produced by Plant

1. Right-click a plant in `Quantity Produced by Plant`.
2. Select Drill through > `Dealer and Truck Detail`.
3. Confirm Page 5 opens filtered to that plant.

### From Page 4: Dealer Service Quality

1. Right-click a dealer in the Top 10 or Bottom 10 dealer service score visual.
2. Select Drill through > `Dealer and Truck Detail`.
3. Confirm Page 5 opens filtered to that dealer.

## Page 5 Required Insights

Write short notes for these items:

1. Dealer with highest revenue.
2. Dealer with high revenue but low service score.
3. Truck model with highest revenue.
4. Truck model with high service cost or frequent service events.
5. Truck model with weak customer rating.
6. Customer industry with the most visible sales records after filtering.

# Interaction and Formatting Steps

## Configure Slicer Behavior

For each page:

1. Select a slicer.
2. Go to `Format` > `Edit interactions`.
3. Confirm the slicer filters every main visual on the page.
4. If a detail table becomes too slow or cluttered, leave it filtered but avoid using highlight mode.

## Configure Drill-down on Date Charts

For each date trend visual:

1. Select the visual.
2. Confirm `DateTable[Date]` is on the X-axis.
3. Use the visual header drill icons.
4. Drill from Year to Quarter to Month.
5. Do not drill to individual day unless needed.

## Apply Number Formatting

Apply these formats:

- `Sales[SalePrice]`: Currency
- `Trucks[BasePrice]`: Currency
- `Production[ProductionCost]`: Currency
- `Service[ServiceCost]`: Currency
- Rating and score fields: 1 or 2 decimal places when averaged
- Count fields: Whole number

## Apply Conditional Formatting

Use conditional formatting in matrices and tables:

1. Select the matrix or table.
2. In Values, open the dropdown for the score or cost field.
3. Select `Conditional formatting`.
4. Choose `Background color`.
5. For scores:
   - Low = red
   - Middle = yellow
   - High = green
6. For costs and downtime:
   - Low = green
   - Middle = yellow
   - High = red

## Final Report Summary to Write

At the end of the analysis, write a short business summary with these exact sections:

### Sales Summary

Include:

- Best region by sales revenue.
- Best truck type by sales revenue.
- Top truck model by sales revenue.
- Most common sales channel.
- Most common payment type.

### Customer Summary

Include:

- Best customer industry by revenue.
- Best company size by revenue.
- Whether larger fleets appear to produce higher sales.

### Production Summary

Include:

- Plant with highest production quantity.
- Plant with highest production cost.
- Most active shift.
- Most active machine line.

### Service Summary

Include:

- Most common service type.
- Part with highest total service cost.
- Dealer with strongest average service score.
- Dealer with weakest average service score.

### Feedback Summary

Include:

- Truck type with highest average customer rating.
- Plant and truck type combination with weakest satisfaction.
- Comment category with lowest average rating.

### Management Actions

Recommend 3 to 5 actions based only on the visuals. Examples:

- Increase focus on high-revenue truck types with strong ratings.
- Investigate high-revenue truck models with weak ratings or high service cost.
- Review dealers with high sales but weak service score.
- Study plants or shifts with high production cost.
- Improve delivery, support, or service processes when satisfaction scores are low.

# Completion Checklist

Before submitting the Power BI case study, confirm:

- The report has exactly 5 pages.
- The page names are exactly:
  - `Executive Overview`
  - `Sales and Customers`
  - `Production Operations`
  - `Service and Feedback`
  - `Dealer and Truck Detail`
- All 8 CSV files are loaded.
- Data types are correct.
- Relationships are created and active.
- `DateTable` is marked as the date table.
- No complex DAX measure table is used.
- Optional calculated columns, if used, are simple and understandable.
- Every chart uses clear field placement and aggregation.
- Slicers filter the visuals correctly.
- Date drill-down works on trend charts.
- Drill-through to Page 5 works from at least 3 source visuals.
- Tables are sorted clearly.
- Conditional formatting is applied to score, cost, or downtime fields where useful.
- The final written insights are based only on visuals, slicers, filters, drill-down, and drill-through.

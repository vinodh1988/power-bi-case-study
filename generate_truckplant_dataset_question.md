# Data Generation Question: Synthetic Truck Plant Dataset

## Task

Using Python and common data-generation libraries, generate a realistic synthetic truck manufacturing, sales, service, and customer feedback dataset as 8 CSV files. The generated files must match the schemas, cardinalities, value domains, and relational rules below.

The final output must be exactly these CSV files:

1. `Customers.csv`
2. `Trucks.csv`
3. `Dealers.csv`
4. `DateTable.csv`
5. `Production.csv`
6. `Sales.csv`
7. `Service.csv`
8. `CustomerFeedback.csv`

Use Python with appropriate libraries such as `pandas`, `numpy`, `faker`, and the standard `datetime` or `calendar` modules. The solution should include a single reproducible Python script, for example `generate_truckplant_dataset.py`, that writes all CSVs to disk.

## Reproducibility Requirements

- Set a fixed random seed for `random`, `numpy`, and `faker`.
- Write CSV files with headers and without index columns.
- Use ISO date strings in `YYYY-MM-DD` format.
- Keep all ID columns as integer surrogate keys starting at 1.
- Generate deterministic row counts:
  - `Customers.csv`: 100 rows
  - `Trucks.csv`: 200 rows
  - `Dealers.csv`: 30 rows
  - `DateTable.csv`: one row per calendar day from `2020-01-01` through `2025-12-31`, inclusive
  - `Production.csv`: 5,000 rows
  - `Sales.csv`: 10,000 rows
  - `Service.csv`: 8,000 rows
  - `CustomerFeedback.csv`: 7,000 rows

## Global Business Context

The dataset represents a truck manufacturer with multiple plants, regional dealers, customers, truck models, production activity, sales transactions, service events, and post-sale feedback.

Use these shared categorical domains consistently:

- Regions: `North`, `South`, `East`, `West`
- Plants: `Plant A`, `Plant B`, `Plant C`
- Trucks are sold and serviced through a mix of dealer and online channels.
- Fact tables must reference valid dimension IDs from the generated dimension tables.

## Reverse-Engineered Source Dataset Summary

The existing dataset has 8 CSVs: 4 dimension-style tables and 4 fact-style tables.

| File | Rows | Role |
|---|---:|---|
| `Customers.csv` | 100 | Customer dimension |
| `Trucks.csv` | 200 | Truck/model dimension |
| `Dealers.csv` | 30 | Dealer dimension |
| `DateTable.csv` | 2,192 | Calendar dimension, currently daily from `2020-01-01` through `2025-12-31` |
| `Production.csv` | 5,000 | Production fact |
| `Sales.csv` | 10,000 | Sales fact |
| `Service.csv` | 8,000 | Service fact |
| `CustomerFeedback.csv` | 7,000 | Feedback fact |

## File 1: Customers.csv

Generate 100 customers.

| Column | Type | Rules |
|---|---:|---|
| `CustomerID` | integer | Sequential primary key from 1 to 100 |
| `CustomerName` | string | Use realistic company/person names from `faker`; allow names with commas where CSV quoting is required |
| `IndustryType` | string | One of `Agriculture`, `Construction`, `Logistics`, `Transport` |
| `Region` | string | One of `North`, `South`, `East`, `West` |
| `CompanySize` | string | One of `Small`, `Medium`, `Large` |
| `AccountSince` | date | Random date from `2015-01-01` through `2024-12-31` |
| `FleetSize` | integer | Random integer from 3 to 100 |
| `CustomerType` | string | One of `Corporate`, `Individual` |
| `Contact` | string | Realistic phone number from `faker.phone_number()` |

Suggested realism:

- Larger company sizes should have higher average `FleetSize`.
- `Corporate` customers should be slightly more common than `Individual` customers in logistics and transport.

## File 2: Trucks.csv

Generate 200 truck models.

| Column | Type | Rules |
|---|---:|---|
| `TruckID` | integer | Sequential primary key from 1 to 200 |
| `ModelName` | string | Format `Truck-1`, `Truck-2`, ... `Truck-200` |
| `TruckType` | string | One of `Light`, `Medium`, `Heavy` |
| `EngineType` | string | One of `Diesel`, `Hybrid`, `Petrol` |
| `FuelType` | string | One of `Diesel`, `Electric`, `Petrol` |
| `LaunchDate` | date | Random date from `2020-01-01` through `2025-12-31` |
| `BasePrice` | decimal | Random monetary value from 50,000.00 to 150,000.00, rounded to 2 decimals |
| `Color` | string | One of `Black`, `Blue`, `Red`, `Silver`, `White` |
| `CapacityTons` | integer | Random integer from 5 to 25 |
| `Plant` | string | One of `Plant A`, `Plant B`, `Plant C` |
| `WarrantyMonths` | integer | One of 24, 36, 48 |

Suggested realism:

- `Heavy` trucks should generally have higher `CapacityTons` and `BasePrice`.
- `Light` trucks should generally have lower `CapacityTons`.
- `Hybrid` engine trucks may have slightly higher average `BasePrice`.

## File 3: Dealers.csv

Generate 30 dealers.

| Column | Type | Rules |
|---|---:|---|
| `DealerID` | integer | Sequential primary key from 1 to 30 |
| `DealerName` | string | Realistic business name from `faker.company()` |
| `Region` | string | One of `North`, `South`, `East`, `West` |
| `OpeningDate` | date | Random date from `2010-01-01` through `2024-12-31` |
| `TotalSales` | integer | Random integer from 50 to 500 |
| `DealerType` | string | One of `Exclusive`, `Multi-brand` |
| `StaffCount` | integer | Random integer from 5 to 45 |

Suggested realism:

- Dealers with higher `TotalSales` should tend to have higher `StaffCount`.
- Ensure every region has at least 5 dealers.

## File 4: DateTable.csv

Generate one row per date from `2020-01-01` through `2025-12-31`.

| Column | Type | Rules |
|---|---:|---|
| `Date` | date | Calendar date |
| `Year` | integer | Calendar year |
| `Month` | integer | Month number, 1 to 12 |
| `Day` | integer | Day of month |
| `DayOfWeek` | integer | Monday = 1 through Sunday = 7 |
| `DayName` | string | Full weekday name, for example `Monday` |
| `MonthName` | string | Full month name, for example `January` |
| `Quarter` | string | `Q1`, `Q2`, `Q3`, or `Q4` |
| `WeekOfYear` | integer | ISO week number |

## File 5: Production.csv

Generate 5,000 production events.

| Column | Type | Rules |
|---|---:|---|
| `ProductionID` | integer | Sequential primary key from 1 to 5000 |
| `TruckID` | integer | Foreign key to `Trucks.TruckID` |
| `Plant` | string | Match the selected truck's `Plant` |
| `ProductionDate` | date | Random date from `2022-01-01` through `2025-12-31`; should not be earlier than the truck's `LaunchDate` where possible |
| `QuantityProduced` | integer | Random integer from 1 to 20 |
| `ProductionCost` | decimal | Random value from 5,000.00 to 50,000.00, rounded to 2 decimals |
| `Shift` | string | One of `Morning`, `Evening`, `Night` |
| `Supervisor` | string | Realistic person name from `faker.name()` |
| `MachineUsed` | string | One of `Line1`, `Line2`, `Line3`, `Line4`, `Line5` |

Suggested realism:

- Production cost should be somewhat correlated with truck type and capacity.
- Ensure every plant, shift, and production line appears multiple times.

## File 6: Sales.csv

Generate 10,000 sales transactions.

| Column | Type | Rules |
|---|---:|---|
| `SaleID` | integer | Sequential primary key from 1 to 10000 |
| `TruckID` | integer | Foreign key to `Trucks.TruckID` |
| `CustomerID` | integer | Foreign key to `Customers.CustomerID` |
| `SaleDate` | date | Random date from `2022-01-01` through `2025-12-31`; should not be earlier than selected truck's `LaunchDate` |
| `SalePrice` | decimal | Derive from truck `BasePrice` with discount/markup; keep approximately 48,000.00 to 165,000.00, rounded to 2 decimals |
| `DealerID` | integer | Foreign key to `Dealers.DealerID` |
| `Region` | string | Prefer the selected dealer's region; optionally align with customer region most of the time |
| `PaymentType` | string | One of `Cash`, `Financing` |
| `Financing` | string | `Yes` or `No`; should usually be `Yes` when `PaymentType` is `Financing` and usually `No` when `PaymentType` is `Cash` |
| `SalesChannel` | string | One of `Dealer`, `Online` |

Suggested realism:

- Corporate customers and customers with larger fleets should have more repeat purchases.
- Heavy trucks should generally sell for higher prices than light trucks.
- Online sales should be less common than dealer sales.

## File 7: Service.csv

Generate 8,000 service events.

| Column | Type | Rules |
|---|---:|---|
| `ServiceID` | integer | Sequential primary key from 1 to 8000 |
| `TruckID` | integer | Foreign key to `Trucks.TruckID` |
| `ServiceDate` | date | Random date from `2022-01-01` through `2025-12-31`; should not be earlier than selected truck's `LaunchDate` |
| `ServiceType` | string | One of `Routine`, `Repair`, `Warranty` |
| `ServiceCost` | decimal | Random value from 100.00 to 5,000.00, rounded to 2 decimals |
| `DealerID` | integer | Foreign key to `Dealers.DealerID` |
| `FeedbackScore` | integer | Random integer from 1 to 10 |
| `DowntimeDays` | integer | Random integer from 0 to 5 |
| `PartReplaced` | string | One of `Brakes`, `Engine`, `Tires`, `Transmission` |

Suggested realism:

- `Routine` services should have lower average `ServiceCost` and shorter downtime.
- `Repair` and `Warranty` events should have higher chances of replacing `Engine` or `Transmission`.
- Higher downtime should slightly reduce `FeedbackScore`.

## File 8: CustomerFeedback.csv

Generate 7,000 customer feedback records.

| Column | Type | Rules |
|---|---:|---|
| `FeedbackID` | integer | Sequential primary key from 1 to 7000 |
| `CustomerID` | integer | Foreign key to `Customers.CustomerID` |
| `TruckID` | integer | Foreign key to `Trucks.TruckID` |
| `FeedbackDate` | date | Random date from `2022-01-01` through `2025-12-31`; should not be earlier than selected truck's `LaunchDate` |
| `Rating` | integer | Random integer from 5 to 10 |
| `Comments` | string | Choose from a realistic list of truck, delivery, support, and service comments |
| `DeliverySatisfaction` | integer | Random integer from 5 to 10 |
| `SupportSatisfaction` | integer | Random integer from 5 to 10 |
| `LikelihoodToRecommend` | integer | Random integer from 6 to 10 |

Use a comment pool similar to:

- `The truck performs excellently in all terrains.`
- `Engine efficiency is superb, very satisfied.`
- `Delivery was delayed by a few days, disappointed.`
- `Cabin comfort could be better for long drives.`
- `Minor issues with brakes, resolved promptly.`
- `Fuel consumption is higher than expected.`
- `Some scratches noticed on delivery, but acceptable.`
- `Excellent service and friendly dealer staff.`
- `Support team responded quickly and professionally.`
- `Would definitely recommend to other logistics companies.`

Suggested realism:

- Higher `DeliverySatisfaction` and `SupportSatisfaction` should generally produce higher `Rating`.
- `LikelihoodToRecommend` should be positively correlated with `Rating`.
- Customers who bought trucks in `Sales.csv` should be more likely to appear in feedback than customers without many purchases.

## Relationship Rules

- `Production.TruckID`, `Sales.TruckID`, `Service.TruckID`, and `CustomerFeedback.TruckID` must exist in `Trucks.TruckID`.
- `Sales.CustomerID` and `CustomerFeedback.CustomerID` must exist in `Customers.CustomerID`.
- `Sales.DealerID` and `Service.DealerID` must exist in `Dealers.DealerID`.
- Date columns in fact tables should exist in `DateTable.Date`.
- Use consistent region, plant, and categorical spelling across all files.
- No primary key column should contain duplicates or nulls.
- Foreign key columns should contain no nulls.

## Validation Requirements

After generating the CSVs, the Python script must run validation checks and print a concise summary:

- Row count for each generated file.
- Duplicate count for each primary key.
- Minimum and maximum date for every date column.
- Confirmation that all foreign keys are valid.
- Category coverage for each categorical column.
- Numeric min/max checks for all bounded numeric columns.

The script should raise an exception if any validation rule fails.

## Expected Deliverable

Provide:

1. A Python script that generates all 8 CSV files.
2. A brief explanation of the generation logic and any correlations used for realism.
3. A validation summary showing that all row counts, keys, date ranges, and categorical domains are correct.

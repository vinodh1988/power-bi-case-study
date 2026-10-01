# Power BI Python Visuals Guide for Truck Plant Dataset

## Important Note About Column Names

The column names used in this guide are based on the current truck plant CSV dataset and are suggestive examples. Actual column names may vary depending on how the data is imported, renamed, transformed, or modeled in Power BI.

Before running any Python visual code:

1. Check the exact column names available in the Python visual's `dataset` object.
2. Update the code if your fields have different names.
3. Make sure the required fields are added to the Python visual's Values well.
4. Remember that Power BI automatically creates a pandas DataFrame named `dataset` for Python visuals.

## When to Use Python Visuals in Power BI

Python visuals are useful when a standard Power BI chart is not flexible enough for the analysis. In this case study, Python can help create richer analytical visuals such as:

1. Sales price distribution by truck type.
2. Service cost versus downtime with service score context.
3. Customer satisfaction comparison across truck types and plants.

These visuals should support analysis, not replace the core Power BI report. Use slicers and filters in Power BI to control the records passed into each Python visual.

## Prerequisites

### Step 1: Install Python

1. Install Python from `https://www.python.org/` or through Anaconda.
2. During installation, enable `Add Python to PATH` if using the standard Python installer.
3. Confirm Python is installed by opening a terminal and running:

```bash
python --version
```

### Step 2: Install Required Python Libraries

Install these libraries:

```bash
pip install pandas matplotlib seaborn numpy
```

### Step 3: Enable Python in Power BI

1. Open Power BI Desktop.
2. Go to `File` > `Options and settings` > `Options`.
3. Select `Python scripting`.
4. Set the detected Python home directory.
5. If Power BI does not detect Python automatically, browse to the Python installation folder.
6. Select `OK`.
7. Restart Power BI Desktop if needed.

### Step 4: Add a Python Visual

1. Open the report page where you want the visual.
2. Select the `Python visual` icon from the Visualizations pane.
3. Power BI will show a script editor.
4. Drag the required fields into the Python visual's Values area.
5. Paste the relevant Python code from this guide.
6. Select the Run icon in the Python script editor.

## Python Visual 1: Sales Price Distribution by Truck Type

### Purpose

This visual shows how sale prices are distributed across different truck types. It helps answer:

- Are heavy trucks generally sold at higher prices?
- Which truck type has the widest price range?
- Are there unusually low or high sale prices?
- Do different truck types overlap in selling price?

### Recommended Report Page

Use this visual on the `Sales and Customers` page.

### Fields to Add to the Python Visual

Add these fields to the Python visual Values well:

| Suggested Field | Source Table | Purpose |
|---|---|---|
| `SalePrice` | `Sales` | Numeric sale price to analyze |
| `TruckType` | `Trucks` | Category used to compare truck types |
| `SalesChannel` | `Sales` | Optional filter/context field |
| `Region` | `Sales` | Optional filter/context field |

Minimum required fields:

- `SalePrice`
- `TruckType`

### What the Code Does

The code:

1. Cleans missing sale price and truck type values.
2. Converts sale price into numeric format.
3. Creates a box plot to compare sale price distribution by truck type.
4. Adds a strip plot so individual transactions are still visible.
5. Formats the chart for readability.

### Sample Python Code

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Power BI automatically provides a DataFrame named dataset.
# Column names may need to be changed to match your model.
df = dataset.copy()

# Suggested column names. Update these if your Power BI fields are named differently.
sale_price_col = 'SalePrice'
truck_type_col = 'TruckType'

# Keep only required columns and remove missing values.
df = df[[sale_price_col, truck_type_col]].dropna()

# Convert sale price to numeric in case Power BI passes it as text.
df[sale_price_col] = pd.to_numeric(df[sale_price_col], errors='coerce')
df = df.dropna(subset=[sale_price_col])

# Set visual style.
sns.set_theme(style='whitegrid')
plt.figure(figsize=(10, 6))

# Box plot shows median, quartiles, spread, and outliers.
sns.boxplot(
    data=df,
    x=truck_type_col,
    y=sale_price_col,
    palette='Set2',
    showfliers=True
)

# Strip plot overlays individual sale transactions.
sns.stripplot(
    data=df,
    x=truck_type_col,
    y=sale_price_col,
    color='black',
    alpha=0.25,
    jitter=True,
    size=3
)

plt.title('Sales Price Distribution by Truck Type', fontsize=14, weight='bold')
plt.xlabel('Truck Type')
plt.ylabel('Sale Price')
plt.xticks(rotation=0)
plt.tight_layout()
plt.show()
```

### How to Interpret the Visual

- A higher box means that truck type generally sells at a higher price.
- A taller box means sale prices vary more for that truck type.
- Dots far above or below the box may represent unusually high or low sale prices.
- If truck types overlap heavily, price differences may not be very strong.

### Useful Power BI Slicers for This Visual

Use these slicers around the Python visual:

- `DateTable[Year]`
- `Sales[Region]`
- `Sales[SalesChannel]`
- `Sales[PaymentType]`
- `Trucks[Plant]`

## Python Visual 2: Service Cost vs Downtime

### Purpose

This visual compares service cost and downtime. It helps identify whether longer downtime is associated with higher service cost and whether some service types or parts are more expensive.

It helps answer:

- Do high downtime service events also cost more?
- Which service types are most expensive?
- Which replaced parts are linked with long downtime?
- Are low service feedback scores connected to higher downtime?

### Recommended Report Page

Use this visual on the `Service and Feedback` page.

### Fields to Add to the Python Visual

Add these fields to the Python visual Values well:

| Suggested Field | Source Table | Purpose |
|---|---|---|
| `ServiceCost` | `Service` | Numeric service cost |
| `DowntimeDays` | `Service` | Numeric downtime duration |
| `FeedbackScore` | `Service` | Score used for point size or interpretation |
| `ServiceType` | `Service` | Color category |
| `PartReplaced` | `Service` | Marker style or tooltip-like category |

Minimum required fields:

- `ServiceCost`
- `DowntimeDays`
- `ServiceType`

### What the Code Does

The code:

1. Removes missing service records.
2. Converts service cost and downtime to numeric values.
3. Creates a scatter plot where each point is a service record.
4. Uses color to show service type.
5. Uses point size to show feedback score if available.
6. Adds a trend line to show the overall relationship between downtime and service cost.

### Sample Python Code

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Power BI automatically provides a DataFrame named dataset.
# Column names may need to be changed to match your model.
df = dataset.copy()

# Suggested column names. Update these if your Power BI fields are named differently.
service_cost_col = 'ServiceCost'
downtime_col = 'DowntimeDays'
feedback_score_col = 'FeedbackScore'
service_type_col = 'ServiceType'

required_cols = [service_cost_col, downtime_col, service_type_col]
optional_cols = [feedback_score_col]

available_cols = [col for col in required_cols + optional_cols if col in df.columns]
df = df[available_cols].dropna(subset=required_cols)

# Convert numeric columns.
df[service_cost_col] = pd.to_numeric(df[service_cost_col], errors='coerce')
df[downtime_col] = pd.to_numeric(df[downtime_col], errors='coerce')

if feedback_score_col in df.columns:
    df[feedback_score_col] = pd.to_numeric(df[feedback_score_col], errors='coerce')
else:
    df[feedback_score_col] = 5

# Remove records that failed numeric conversion.
df = df.dropna(subset=[service_cost_col, downtime_col, feedback_score_col])

sns.set_theme(style='whitegrid')
plt.figure(figsize=(10, 6))

# Scatter plot with service type as color and feedback score as size.
sns.scatterplot(
    data=df,
    x=downtime_col,
    y=service_cost_col,
    hue=service_type_col,
    size=feedback_score_col,
    sizes=(30, 180),
    alpha=0.7,
    edgecolor='black',
    linewidth=0.4
)

# Regression trend line for the full dataset.
sns.regplot(
    data=df,
    x=downtime_col,
    y=service_cost_col,
    scatter=False,
    color='black',
    line_kws={'linewidth': 2, 'linestyle': '--'}
)

plt.title('Service Cost vs Downtime', fontsize=14, weight='bold')
plt.xlabel('Downtime Days')
plt.ylabel('Service Cost')
plt.legend(title='Service Type / Score', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.tight_layout()
plt.show()
```

### How to Interpret the Visual

- Points farther right have longer downtime.
- Points higher up have higher service cost.
- A rising trend line suggests higher downtime is associated with higher service cost.
- A cluster of expensive repair points may indicate a service risk area.
- If low feedback scores appear around high downtime, the business should investigate service delays.

### Useful Power BI Slicers for This Visual

Use these slicers around the Python visual:

- `DateTable[Year]`
- `Service[ServiceType]`
- `Service[PartReplaced]`
- `Dealers[Region]`
- `Trucks[TruckType]`
- `Trucks[Plant]`

## Python Visual 3: Customer Satisfaction Heatmap

### Purpose

This visual compares average customer satisfaction across truck plants and truck types. It helps quickly identify combinations with strong or weak customer experience.

It helps answer:

- Which plant and truck type combination has the highest average rating?
- Which combination has the weakest rating?
- Are customer ratings consistent across plants?
- Are some truck types rated better regardless of plant?

### Recommended Report Page

Use this visual on the `Service and Feedback` page or the `Executive Overview` page.

### Fields to Add to the Python Visual

Add these fields to the Python visual Values well:

| Suggested Field | Source Table | Purpose |
|---|---|---|
| `Plant` | `Trucks` | Row category |
| `TruckType` | `Trucks` | Column category |
| `Rating` | `CustomerFeedback` | Main satisfaction score |
| `DeliverySatisfaction` | `CustomerFeedback` | Optional supporting score |
| `SupportSatisfaction` | `CustomerFeedback` | Optional supporting score |

Minimum required fields:

- `Plant`
- `TruckType`
- `Rating`

### What the Code Does

The code:

1. Removes records where plant, truck type, or rating is missing.
2. Converts rating to numeric format.
3. Groups the data by plant and truck type.
4. Calculates average rating for each plant and truck type combination.
5. Creates a heatmap where darker or stronger colors represent higher average ratings.
6. Writes the average rating value inside each heatmap cell.

### Sample Python Code

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Power BI automatically provides a DataFrame named dataset.
# Column names may need to be changed to match your model.
df = dataset.copy()

# Suggested column names. Update these if your Power BI fields are named differently.
plant_col = 'Plant'
truck_type_col = 'TruckType'
rating_col = 'Rating'

# Keep required columns and clean data.
df = df[[plant_col, truck_type_col, rating_col]].dropna()
df[rating_col] = pd.to_numeric(df[rating_col], errors='coerce')
df = df.dropna(subset=[rating_col])

# Create grouped average rating table.
summary = (
    df.groupby([plant_col, truck_type_col], as_index=False)[rating_col]
      .mean()
)

# Pivot data for heatmap format.
heatmap_data = summary.pivot(
    index=plant_col,
    columns=truck_type_col,
    values=rating_col
)

sns.set_theme(style='white')
plt.figure(figsize=(9, 5))

sns.heatmap(
    heatmap_data,
    annot=True,
    fmt='.2f',
    cmap='YlGnBu',
    linewidths=0.5,
    linecolor='white',
    cbar_kws={'label': 'Average Rating'}
)

plt.title('Average Customer Rating by Plant and Truck Type', fontsize=14, weight='bold')
plt.xlabel('Truck Type')
plt.ylabel('Plant')
plt.tight_layout()
plt.show()
```

### How to Interpret the Visual

- Higher values indicate stronger customer ratings.
- Lower values indicate combinations that may need investigation.
- Compare rows to see whether one plant performs better across truck types.
- Compare columns to see whether one truck type performs better across plants.
- If one cell is much lower than the others, drill into that plant and truck type using normal Power BI filters.

### Useful Power BI Slicers for This Visual

Use these slicers around the Python visual:

- `DateTable[Year]`
- `Customers[IndustryType]`
- `Customers[Region]`
- `Trucks[FuelType]`
- `Trucks[EngineType]`

## Troubleshooting Python Visuals in Power BI

### Problem: The visual says a column does not exist

Cause:

- The field name in the code does not match the field name passed by Power BI.

Fix:

1. Check the Values well of the Python visual.
2. Confirm the exact field names.
3. Update variables such as `sale_price_col`, `truck_type_col`, or `rating_col` in the code.

### Problem: The visual is blank

Possible causes:

- Required fields were not added to the Values well.
- Slicers filtered the data down to zero rows.
- Numeric columns were imported as text and could not be converted.
- The Python script has an error.

Fix:

1. Clear slicers.
2. Add the minimum required fields only.
3. Confirm data types in Power Query.
4. Run the visual again.

### Problem: The visual is too crowded

Fix:

- Use slicers to reduce the data.
- Filter to one year, region, plant, truck type, or service type.
- Increase the visual size on the canvas.
- Use Top N filtering in Power BI before sending fields to the Python visual.

### Problem: The Python visual does not react as expected

Remember:

- Python visuals receive filtered data from Power BI.
- They respond to slicers and page filters.
- They do not support Power BI cross-highlighting in the same way as native visuals.
- They render as static images after the Python script runs.

## Recommended Placement in the 5-Page Case Study

| Python Visual | Best Page | Reason |
|---|---|---|
| Sales Price Distribution by Truck Type | `Sales and Customers` | Explains sale price spread and truck type pricing behavior |
| Service Cost vs Downtime | `Service and Feedback` | Shows relationship between service delay, cost, type, and score |
| Customer Satisfaction Heatmap | `Service and Feedback` or `Executive Overview` | Quickly identifies weak satisfaction areas by plant and truck type |

## Final Notes

These Python visuals are intended to add analytical depth. The rest of the Power BI report can still use normal visuals, slicers, drill-down, drill-through, tables, and matrices.

Use Python visuals only where they explain something that is harder to show clearly with standard Power BI visuals.

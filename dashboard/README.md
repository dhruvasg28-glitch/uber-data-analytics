# Uber Ride Booking Analytics Dashboard

## 1. Project Overview

This project is an **Uber Ride Booking Analytics Dashboard** created using **Microsoft Power BI**. The purpose of the project is to convert raw Uber ride-booking data into an interactive and easy-to-understand business intelligence dashboard.

The project follows a complete Power BI workflow:

**Raw Dataset → Power Query Cleaning → Data Modeling → DAX Measures → Dashboard Design → Interactive Analysis**

The source dataset contains **150,000 ride-booking records and 21 columns**. The data contains information about booking status, vehicle type, pickup and drop locations, booking value, ride distance, cancellation reasons, ratings, payment methods, and other operational metrics.

The final dashboard is organized into five major analysis areas:

1. **Overall**
2. **Vehicle Type**
3. **Revenue**
4. **Cancellation**
5. **Ratings**

The dashboard also uses date filters and interactive navigation so that users can move between different analytical views.

---

## 2. Dataset Information

### Dataset Name

`ncr_ride_bookings.csv`

### Dataset Size

- **Rows:** 150,000
- **Columns:** 21
- **Data type:** Structured tabular CSV data
- **Analysis period:** 2024

### Main Columns

| Column | Description |
|---|---|
| Date | Date on which the booking was created |
| Time | Time of booking |
| Booking ID | Unique identifier for a booking |
| Booking Status | Status of the ride/booking |
| Customer ID | Unique identifier for the customer |
| Vehicle Type | Vehicle selected for the ride |
| Pickup Location | Starting location |
| Drop Location | Destination location |
| Avg VTAT | Average Vehicle Time of Arrival |
| Avg CTAT | Average Customer Time of Arrival |
| Cancelled Rides by Customer | Indicates customer-side cancellation |
| Reason for cancelling by Customer | Reason provided for customer cancellation |
| Cancelled Rides by Driver | Indicates driver-side cancellation |
| Driver Cancellation Reason | Reason provided for driver cancellation |
| Incomplete Rides | Indicates incomplete rides |
| Incomplete Rides Reason | Reason for incomplete ride |
| Booking Value | Monetary value of the booking |
| Ride Distance | Distance travelled |
| Driver Ratings | Rating given to the driver |
| Customer Rating | Rating associated with the customer |
| Payment Method | Method used for payment |

---

# 3. Project Workflow

The dashboard was developed in several stages.

### Step 1: Import the Dataset

The raw CSV dataset was imported into **Power BI Desktop**.

The dataset was first inspected to understand:

- Number of records
- Available columns
- Data types
- Missing values
- Booking statuses
- Vehicle categories
- Cancellation information
- Revenue-related fields
- Rating fields
- Payment methods

At this stage, the data was still in its raw form and required preparation before it could be used reliably for analysis.

---

# 4. Data Cleaning Using Power Query Editor

## 4.1 Why Power Query Was Used

Power Query Editor is the data preparation layer in Power BI. It allows raw data to be cleaned and transformed before it reaches the data model.

Cleaning the data before creating visuals is important because dashboards depend on accurate data types, consistent categories, and properly handled missing values.

The general process used in this project was:

**Power BI → Transform Data → Power Query Editor → Clean/Transform Data → Close & Apply**

---

## 4.2 Understanding the Raw Data

The dataset contains different types of information:

- Text fields such as Vehicle Type and Booking Status
- Date fields such as Date
- Time information
- Numerical fields such as Booking Value and Ride Distance
- Rating fields
- Cancellation indicators
- Cancellation reason fields

Before creating measures or visuals, these fields need to be correctly interpreted by Power BI.

---

## 4.3 Data Type Preparation

One of the important Power Query steps is assigning appropriate data types.

Examples:

- `Date` → Date
- `Time` → Time
- `Booking Value` → Decimal/Number
- `Ride Distance` → Decimal/Number
- `Driver Ratings` → Decimal/Number
- `Customer Rating` → Decimal/Number
- Booking and customer IDs → Text
- Booking Status → Text
- Vehicle Type → Text
- Payment Method → Text

Correct data types are essential because Power BI uses them when performing calculations, filtering, grouping, sorting, and visualization.

For example, if Booking Value is treated as text, Power BI cannot correctly calculate total revenue.

---

## 4.4 Handling Missing Values

The dataset contains blank values in several columns.

This is expected because not every field applies to every booking.

For example:

- A completed ride may have a Booking Value.
- A "No Driver Found" booking may not have a Booking Value.
- A cancellation reason is relevant only when a cancellation occurs.
- Ratings may not be available for every booking.

Therefore, missing values were considered according to the meaning of each column rather than blindly replacing every blank with zero.

This is important because replacing every blank with zero can create incorrect business conclusions.

---

## 4.5 Cleaning Text and Category Fields

Categorical fields such as:

- Booking Status
- Vehicle Type
- Pickup Location
- Drop Location
- Payment Method
- Cancellation Reasons

need consistent text values.

Text preparation helps prevent situations where visually similar categories are treated as different categories because of unnecessary spaces or inconsistent formatting.

For example:

`UPI`

and

` UPI `

should represent the same payment method.

---

## 4.6 Preparing Numerical Fields

Numerical columns were prepared so that Power BI could perform aggregation and analysis.

Important numerical fields include:

- Booking Value
- Ride Distance
- Avg VTAT
- Avg CTAT
- Driver Ratings
- Customer Rating
- Cancellation indicators
- Incomplete Ride indicators

These fields can then be used with aggregation functions such as:

- SUM
- COUNT
- DISTINCTCOUNT
- AVERAGE
- DIVIDE

---

## 4.7 Date Preparation

The Date field is especially important because the dashboard contains date slicers.

The date field allows the user to select a period such as:

**01/01/2024 → 30/12/2024**

and dynamically update the dashboard.

Date-based filtering is used across the dashboard to make the analysis interactive.

---

## 4.8 Applying the Transformations

After the required transformations were completed in Power Query:

**Home → Close & Apply**

was used to load the cleaned data into the Power BI data model.

The cleaned data then became the foundation for the next stage: **data modeling and DAX calculations**.

---

# 5. Data Modeling

## 5.1 What Is Data Modeling?

Data modeling is the process of preparing the data structure so that Power BI can efficiently perform calculations and connect different fields.

In this project, the cleaned Uber booking dataset was used as the main analytical table.

The data model provides the foundation for:

- Measures
- Calculated analysis
- Filtering
- Aggregation
- Visual interactions
- Dashboard performance

---

# 6. Creating Measures Using DAX

After cleaning the data, measures were created using **DAX (Data Analysis Expressions)**.

Measures are dynamic calculations. Unlike simple columns, measures are calculated according to the current filter context.

For example, if the user selects a particular date range, the same measure can automatically calculate the result only for that selected period.

---

## 6.1 Total Bookings

A basic booking measure can be created as:

```DAX
Total Bookings =
COUNT('ncr_ride_bookings'[Booking ID])
```

This measure calculates the total number of booking records.

The dashboard uses this type of calculation for KPI cards such as:

**Total Bookings**

---

## 6.2 Successful Bookings

Successful bookings can be calculated by counting bookings whose status represents completed rides.

Example:

```DAX
Success Bookings =
CALCULATE(
    COUNT('ncr_ride_bookings'[Booking ID]),
    'ncr_ride_bookings'[Booking Status] = "Completed"
)
```

This provides the number of successfully completed bookings.

---

## 6.3 Total Booking Value

A revenue/value measure can be calculated using:

```DAX
Total Booking Value =
SUM('ncr_ride_bookings'[Booking Value])
```

This measure is useful for:

- Revenue analysis
- Vehicle comparison
- Payment method analysis
- Customer contribution analysis

---

## 6.4 Average Ride Distance

The average ride distance can be calculated using:

```DAX
Average Ride Distance =
AVERAGE('ncr_ride_bookings'[Ride Distance])
```

This helps understand the typical distance travelled per booking.

---

## 6.5 Total Distance

```DAX
Total Distance =
SUM('ncr_ride_bookings'[Ride Distance])
```

This measure provides the total recorded ride distance.

---

## 6.6 Cancellation Rate

A cancellation rate can be calculated using a ratio between cancelled bookings and total bookings.

Example:

```DAX
Cancellation Rate =
DIVIDE(
    [Cancelled Bookings],
    [Total Bookings],
    0
)
```

The `DIVIDE()` function is useful because it safely handles cases where the denominator is zero.

---

## 6.7 Average Customer Rating

```DAX
Average Customer Rating =
AVERAGE('ncr_ride_bookings'[Customer Rating])
```

This measure supports the customer rating analysis.

---

## 6.8 Average Driver Rating

```DAX
Average Driver Rating =
AVERAGE('ncr_ride_bookings'[Driver Ratings])
```

This measure supports driver performance analysis.

> **Note:** The exact DAX formulas can be adjusted depending on the final measure names and business rules used in the PBIX file. The examples above document the analytical logic used by the dashboard.

---

# 7. Dashboard Design

The dashboard was designed as a multi-page analytical interface rather than a single static report.

A navigation panel is provided on the left side of the dashboard.

The main sections are:

- Overall
- Vehicle Type
- Revenue
- Cancellation
- Ratings

This structure allows users to move from high-level performance to detailed operational analysis.

---

# 8. Overall Dashboard

## Purpose

The **Overall** page provides a high-level summary of Uber booking performance.

It is intended to answer questions such as:

- How many bookings were recorded?
- How many rides were completed?
- What proportion of bookings were cancelled?
- How did booking volume change over time?
- What is the overall booking-status distribution?

---

## Date Filter

The page contains a date slicer that allows the user to select a specific time period.

This makes the dashboard dynamic.

For example, when the selected date range changes, the KPI cards and charts update according to the selected period.

---

## Total Bookings KPI

The **Total Bookings** card provides an immediate view of the number of bookings.

In the dashboard view, the total is displayed approximately as:

**148.77K**

The `K` represents thousands.

This KPI is useful as the main indicator of booking activity.

---

## Booking Status Breakdown

A pie/donut-style visualization shows the distribution of booking statuses.

The dashboard analyzes categories such as:

- Completed
- Cancelled by Driver
- Cancelled by Customer
- No Driver Found

This visual helps identify how the total booking volume is distributed between successful and unsuccessful booking outcomes.

---

## Ride Volume Over Time

The time-series chart shows the number of bookings by month.

The chart helps identify:

- High-volume months
- Low-volume months
- Seasonal changes
- Trends in booking activity

For example, users can compare January with February, March, and other months to understand changes in demand.

---

# 9. Vehicle Type Dashboard

## Purpose

The Vehicle Type page compares performance across different vehicle categories.

The dashboard includes vehicle categories such as:

- Go Mini
- Go Sedan
- UberXL
- Premier Sedan
- Auto
- Bike/eBike

---

## Key Metrics

The vehicle analysis contains measures such as:

### Total Booking Value

Shows the total monetary booking value associated with each vehicle type.

### Success Booking Value

Shows the booking value associated with successfully completed rides.

### Average Distance Travelled

Shows the average distance for rides associated with each vehicle category.

### Total Distance Travelled

Shows the total ride distance accumulated by each vehicle category.

---

## Business Use

This page can help answer questions such as:

- Which vehicle type generates the highest booking value?
- Which vehicle category has the highest total distance?
- Which vehicle is used most heavily?
- How does successful booking value compare with total booking value?
- Are premium vehicle categories contributing significant value?

---

## Example Dashboard Interpretation

From the displayed dashboard:

- **Auto** has a high total booking value and high total distance.
- **eBike/Bike** also contributes a large booking value and distance.
- **Go Mini** shows strong booking value and successful booking value.
- **UberXL** has a lower total value compared with the higher-volume vehicle categories.

These comparisons allow the business to understand vehicle-level demand and contribution.

---

# 10. Revenue Dashboard

## Purpose

The Revenue page focuses on the monetary side of the ride-booking business.

It is useful for understanding:

- Revenue contribution
- Payment behavior
- Customer contribution
- Ride-distance distribution

---

## Ride Distance Distribution

The dashboard contains a bar chart showing ride distance distribution by day.

This helps identify how ride distance varies throughout the month.

The analysis can be useful for understanding:

- Daily travel demand
- Short-distance vs. long-distance activity
- Changes in operational workload

---

## Revenue by Payment Method

A bar chart compares booking value by payment method.

Payment categories include:

- UPI
- Cash
- Uber Wallet
- Credit Card
- Debit Card

In the dashboard, **UPI represents the largest booking value contribution** among the displayed payment methods.

This can help the business understand customer payment preferences and digital-payment adoption.

---

## Top 5 Customers

The dashboard also identifies the **Top 5 Customers** based on booking value.

This analysis helps answer:

- Which customers generate the highest booking value?
- How much value is contributed by the highest-value customers?
- Who are the most valuable customers in the selected period?

This information can support customer retention and loyalty strategies.

---

# 11. Cancellation Dashboard

## Purpose

The Cancellation page focuses on ride cancellations and helps identify why bookings fail to become successful rides.

The dashboard includes:

- Customer cancellations
- Driver cancellations
- Cancellation reasons
- Total bookings
- Successful bookings
- Cancelled bookings
- Cancellation rate

---

## Customer Cancellation Analysis

A pie chart shows the distribution of customer cancellation reasons.

Examples of reasons represented in the data include:

- Wrong Address
- Change of Plans
- Driver is Not Moving Towards Pickup
- Driver Asked to Cancel
- AC is Not Working

The chart allows the business to identify which customer-side issues contribute most to cancellations.

---

## Driver Cancellation Analysis

A second pie chart analyzes driver-side cancellation reasons.

Examples include categories related to:

- Customer-related issues
- Customer not showing up
- Personal/car-related issues
- Other operational reasons

This helps identify problems occurring from the driver's side of the platform.

---

## Cancellation KPI Cards

The page provides KPI cards for:

### Total Bookings

The total number of bookings in the selected period.

### Success Bookings

The number of successfully completed bookings.

### Cancelled Bookings

The number of bookings that were cancelled.

### Cancellation Rate

The percentage of bookings that were cancelled.

The displayed dashboard shows a cancellation rate of approximately **25%** for the selected analysis period.

---

## Business Importance

Cancellation analysis is important because a high cancellation rate can result in:

- Lost revenue
- Poor customer experience
- Driver dissatisfaction
- Lower operational efficiency
- Reduced successful ride conversion

By identifying cancellation reasons, management can investigate the underlying operational problems.

---

# 12. Ratings Dashboard

## Purpose

The Ratings page evaluates service quality through:

- Customer Ratings
- Driver Ratings

The dashboard compares ratings across vehicle types.

---

## Customer Ratings

The dashboard presents average customer ratings for each vehicle category.

The displayed values are approximately around **4.40–4.41** across the vehicle categories.

This indicates relatively consistent customer-rating performance among the displayed vehicle types.

---

## Driver Ratings

The dashboard also compares driver ratings.

The displayed values are approximately around **4.23–4.24**.

This provides an overview of driver-side service quality.

---

## Business Questions

The Ratings page helps answer:

- Which vehicle type has the highest customer rating?
- Which vehicle type has the highest driver rating?
- Are ratings consistent across vehicle categories?
- Is there a noticeable difference between customer and driver rating patterns?

---

# 13. Interactive Features

One of the main advantages of the Power BI dashboard is interactivity.

## Date Slicers

Date slicers allow users to select the analysis period.

When the date range changes, connected visuals update automatically.

---

## Navigation Buttons

The left-side navigation allows users to move between:

**Overall → Vehicle Type → Revenue → Cancellation → Ratings**

This creates a structured dashboard experience.

---

## Cross Filtering

Power BI visuals can interact with each other.

For example, selecting a vehicle category can affect related visuals depending on the configured visual interactions.

This allows users to explore the data instead of viewing only fixed charts.

---

# 14. Dashboard Design and User Experience

The dashboard uses a consistent visual design across the pages.

Important design elements include:

- Left-side navigation
- KPI cards
- Rounded containers
- Consistent typography
- Vehicle icons
- Charts and tables
- Date slicers
- Clear page titles
- Separate analytical sections

The navigation panel helps users understand where they are within the report.

The use of KPI cards makes important numbers immediately visible, while charts provide deeper analysis.

---

# 15. Key Business Insights

The dashboard can be used to identify several important patterns.

### Booking Performance

The dataset contains a very large number of booking records, indicating substantial ride activity.

### Completed vs Cancelled

The Booking Status analysis shows that completed rides form the largest category, while cancellations and "No Driver Found" bookings account for a significant portion of the remaining activity.

### Vehicle Performance

Different vehicle categories contribute differently to total booking value and total distance.

Auto and eBike/Bike show strong contribution in the displayed vehicle analysis.

### Payment Behavior

UPI is the largest payment-method category by booking value in the displayed Revenue page.

### Cancellation

The dashboard shows approximately a **25% cancellation rate** for the displayed overall period, making cancellation reduction an important operational opportunity.

### Customer Value

The Top 5 Customers visual highlights customers contributing the highest booking value.

### Ratings

Customer ratings are around **4.4**, while driver ratings are around **4.23–4.24**, indicating generally strong but slightly different rating patterns.

---

# 16. Why This Project Is Useful

This project demonstrates how raw operational data can be converted into a business intelligence solution.

The project covers important Power BI skills:

- Data importing
- Power Query
- Data cleaning
- Data type management
- Data modeling
- DAX measures
- KPI creation
- Interactive filtering
- Data visualization
- Dashboard design
- Business analysis

It therefore demonstrates not only visualization skills but also the complete process of preparing and analyzing business data.

---

# 17. Tools and Technologies

### Microsoft Power BI Desktop

Used for:

- Data import
- Power Query transformation
- Data modeling
- DAX calculations
- Visualization
- Dashboard development

### Power Query Editor

Used for:

- Data cleaning
- Transformation
- Data type preparation
- Handling missing values
- Preparing fields for analysis

### DAX

Used for:

- Dynamic measures
- KPI calculations
- Aggregations
- Ratios
- Performance calculations

### CSV

Used as the original source data format.

---

# 18. Project Files

The project package contains:

```text
Uber-Dashboard/
│
├── ncr_ride_bookings.csv
├── Uber.pbix
├── Dasboard.gif
└── README.md
```

### `ncr_ride_bookings.csv`

Original Uber ride-booking dataset used as the data source.

### `Uber.pbix`

Power BI project containing:

- Power Query transformations
- Data model
- DAX measures
- Dashboard pages
- Visualizations
- Filters and navigation

### `Dasboard.gif`

Animated demonstration/preview of the completed dashboard.

### `README.md`

Documentation explaining the project, data preparation, modeling, measures, dashboard pages, and analytical insights.

---

# 19. End-to-End Process Summary

The complete project can be summarized as follows:

```text
Raw Uber Dataset
       ↓
Import into Power BI
       ↓
Power Query Editor
       ↓
Data Cleaning & Transformation
       ↓
Close & Apply
       ↓
Data Modeling
       ↓
Create DAX Measures
       ↓
Build KPIs and Visuals
       ↓
Create Dashboard Pages
       ↓
Add Slicers & Navigation
       ↓
Analyze Business Performance
```

---

# 20. Conclusion

The Uber Ride Booking Analytics Dashboard transforms a raw dataset containing **150,000 booking records** into an interactive Power BI business intelligence solution.

The project begins with data preparation in Power Query, where the raw dataset is cleaned and structured for analysis. After the cleaning stage, the data is loaded into the Power BI model and analytical measures are created using DAX.

The final dashboard provides multiple perspectives on Uber operations, including overall booking performance, vehicle-type performance, revenue and payment behavior, cancellation analysis, and service ratings.

The dashboard demonstrates how Power BI can be used to move from **raw data to meaningful business insights**. Instead of manually examining thousands of records, users can interact with KPIs, charts, tables, slicers, and navigation controls to quickly understand important patterns and make data-driven decisions.

---

## Author / Project

**Project:** Uber Ride Booking Analytics Dashboard  
**Tool:** Microsoft Power BI  
**Dataset:** NCR Uber Ride Bookings  
**Records:** 150,000  
**Analysis Year:** 2024  


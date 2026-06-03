
# Global Superstore Sales Analysis

## Project Overview
This project involves a comprehensive analysis of sales data from a global superstore. The primary objective is to understand sales and profit distributions, identify key performing categories and regions, and critically, to pinpoint and analyze transactions that exhibit high sales but yield low or negative profit. The insights derived aim to help in optimizing pricing strategies, managing operational costs, and improving overall business profitability.

## Data Source
The analysis uses the `Global_Superstore2.xlsx` dataset, which contains detailed transaction records including sales figures, profit, product information, customer details, and geographical data.

## Libraries Used
-   `pandas` for data manipulation and analysis.
-   `numpy` for numerical operations.
-   `matplotlib.pyplot` for data visualization.

## Methodology

### 1. Data Loading and Initial Exploration
-   Loaded the `Global_Superstore2.xlsx` file into a pandas DataFrame.
-   Checked the dimensions of the dataset (`data.shape`).
-   Obtained a summary of the DataFrame including data types and non-null counts (`data.info()`).
-   Calculated key overall metrics such as Total Sales, Total Profit, Total Orders, Total Discount, and Total Customers.

### 2. Data Cleaning
-   Identified and handled missing values, specifically dropping the 'Postal Code' column due to a high number of missing entries.
-   Checked for and confirmed the absence of duplicate rows in the dataset.
-   Generated summary descriptive statistics for numerical columns (`data.describe()`).
-   Inspected unique values for all object (categorical) columns to understand their diversity and potential inconsistencies.

### 3. Exploratory Data Analysis (EDA)
-   **Distribution of Sales and Profit**: Visualized the distributions using histograms to understand their spread and identify outliers. Noted skewed distributions and the presence of negative profit transactions.
-   **Sales and Profit by Category**: Compared total sales and profit across product categories (Technology, Furniture, Office Supplies) using bar plots.
-   **Sales and Profit by Region**: Analyzed total sales and profit by geographical region using bar plots.
-   **Relationship between Sales and Profit**: Used a scatter plot to visualize the correlation, observing a general positive trend but also significant transactions with high sales and low/negative profit.

### 4. Analysis of Unprofitable High-Sales Transactions
-   **Identification**: Filtered transactions where sales were above the 95th percentile and profit was zero or negative.
-   **Characteristics Examination**: Examined the categories, sub-categories, regions, average discount, and average quantity within these unprofitable transactions.
-   **Visualizations**: Created scatter plots to highlight these transactions and bar plots to show their distribution across categories, sub-categories, and regions.

### 5. Deep Dive into Unprofitability Drivers
-   **Pricing and Cost Structures**: Investigated average sales, profit, and discount for 'Furniture' (specifically 'Tables' and 'Chairs') and 'Technology' categories within the unprofitable transactions.
-   **Regional Factors**: Examined average sales, profit, and shipping costs for 'Central', 'Southeast Asia', and 'Oceania' regions, which showed a higher concentration of unprofitable transactions.
-   **Other Contributing Factors**: Explored 'Order Priority' and 'Ship Mode' distributions, and the mean 'Quantity' for these transactions.

## Key Findings and Insights

-   **Overall Profitability**: Despite high overall sales, a significant portion of transactions, particularly those with high sales values, resulted in losses, indicating that high volume doesn't always equate to profitability.
-   **Category Performance**: 'Technology' is the leading category in both sales and profit. However, 'Furniture' and 'Office Supplies' also contribute significantly, with specific issues identified within 'Furniture'.
-   **Regional Performance**: The 'Central' region shows the highest sales and profit but is also a hotspot for unprofitable high-sales transactions, alongside 'Southeast Asia' and 'Oceania'.
-   **Drivers of Unprofitability (High-Sales Transactions)**:
    -   **Discounts**: Aggressive discounting, especially in the 'Furniture' category (particularly for 'Tables' and 'Chairs'), is a primary factor eroding profit margins.
    -   **Shipping Costs**: Substantial shipping costs in 'Central', 'Southeast Asia', and 'Oceania' regions contribute significantly to losses.
    -   **Product Focus**: 'Tables' and 'Chairs' in the 'Furniture' category are frequently involved in unprofitable transactions, pointing to potential product-specific cost or pricing challenges.
    -   **Order Fulfillment**: A high number of unprofitable transactions had 'Medium' order priority and used 'Standard Class' shipping, suggesting potential operational inefficiencies in these fulfillment methods.

## Conclusion and Recommendations
The analysis reveals critical areas where high sales do not translate into profit, primarily driven by excessive discounts, high shipping costs in certain regions, and specific product-category challenges. To improve overall business performance, further investigation into pricing strategies, logistics cost optimization, and product-specific profitability is recommended.

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load Dataset
data = pd.read_excel('/content/Global_Superstore2.xlsx')

# Data Cleaning
data.drop('Postal Code', axis=1, inplace=True)
duplicate = data.duplicated().sum()
if duplicate > 0:
    print(f"Found {duplicate} duplicate rows, dropping them.")
    data.drop_duplicates(inplace=True)

# Overall Metrics
total_sales = data['Sales'].sum()
total_profit = data['Profit'].sum()
total_orders = data['Order ID'].nunique()
total_discount = data['Discount'].sum()
total_customers = data['Customer ID'].nunique()

print("\n--- Overall Business Metrics ---")
print(f"Total Sales: {total_sales:.2f}")
print(f"Total Profit: {total_profit:.2f}")
print(f"Total Orders: {total_orders}")
print(f"Total Discount: {total_discount:.2f}")
print(f"Total Customers: {total_customers}")

# --- Exploratory Data Analysis (EDA) ---

# Distribution of Sales and Profit
plt.figure(figsize=(12, 5))
plt.subplot(1, 2, 1)
plt.hist(data['Sales'], bins=50, color='skyblue')
plt.title('Distribution of Sales')
plt.xlabel('Sales')
plt.ylabel('Frequency')

plt.subplot(1, 2, 2)
plt.hist(data['Profit'], bins=50, color='lightcoral')
plt.title('Distribution of Profit')
plt.xlabel('Profit')
plt.ylabel('Frequency')
plt.tight_layout()
plt.show()

# Sales and Profit by Category
category_analysis = data.groupby('Category')[['Sales', 'Profit']].sum().reset_index()
fig, axes = plt.subplots(1, 2, figsize=(14, 6))
axes[0].bar(category_analysis['Category'], category_analysis['Sales'], color='skyblue')
axes[0].set_title('Total Sales by Category')
axes[0].set_xlabel('Category')
axes[0].set_ylabel('Total Sales')
axes[0].tick_params(axis='x', rotation=45)
axes[1].bar(category_analysis['Category'], category_analysis['Profit'], color='lightcoral')
axes[1].set_title('Total Profit by Category')
axes[1].set_xlabel('Category')
axes[1].set_ylabel('Total Profit')
axes[1].tick_params(axis='x', rotation=45)
plt.tight_layout()
plt.show()

# Sales and Profit by Region
region_analysis = data.groupby('Region')[['Sales', 'Profit']].sum().reset_index()
fig, axes = plt.subplots(1, 2, figsize=(18, 6))
axes[0].bar(region_analysis['Region'], region_analysis['Sales'], color='skyblue')
axes[0].set_title('Total Sales by Region')
axes[0].set_xlabel('Region')
axes[0].set_ylabel('Total Sales')
axes[0].tick_params(axis='x', rotation=45)
axes[1].bar(region_analysis['Region'], region_analysis['Profit'], color='lightcoral')
axes[1].set_title('Total Profit by Region')
axes[1].set_xlabel('Region')
axes[1].set_ylabel('Total Profit')
axes[1].tick_params(axis='x', rotation=45)
plt.tight_layout()
plt.show()

# Relationship between Sales and Profit
plt.figure(figsize=(10, 6))
plt.scatter(data['Sales'], data['Profit'], alpha=0.5)
plt.title('Relationship between Sales and Profit')
plt.xlabel('Sales')
plt.ylabel('Profit')
plt.show()

# --- Analysis of Unprofitable High-Sales Transactions ---
high_sales_threshold = data['Sales'].quantile(0.95)
high_sales_low_profit = data[(data['Profit'] <= 0) & (data['Sales'] >= high_sales_threshold)]

print("\n--- High Sales, Low/Negative Profit Transactions ---")
print(f"Number of transactions with high sales (> {high_sales_threshold:.2f}) and low/negative profit: {len(high_sales_low_profit)}")

# Characteristics of Unprofitable Transactions
category_counts = high_sales_low_profit['Category'].value_counts()
sub_category_counts = high_sales_low_profit['Sub-Category'].value_counts()
region_counts = high_sales_low_profit['Region'].value_counts()
discount_mean = high_sales_low_profit['Discount'].mean()
quantity_mean = high_sales_low_profit['Quantity'].mean()

print("\nValue counts for Category in high_sales_low_profit:")
print(category_counts)
print("\nValue counts for Sub-Category in high_sales_low_profit:")
print(sub_category_counts)
print("\nValue counts for Region in high_sales_low_profit:")
print(region_counts)
print(f"\nMean of Discount in high_sales_low_profit: {discount_mean:.4f}")
print(f"Mean of Quantity in high_sales_low_profit: {quantity_mean:.4f}")

# Visualize Unprofitable Transactions
plt.figure(figsize=(10, 6))
plt.scatter(high_sales_low_profit['Sales'], high_sales_low_profit['Profit'], alpha=0.6, color='red')
plt.title('Sales vs. Profit for High Sales, Low/Negative Profit Transactions')
plt.xlabel('Sales')
plt.ylabel('Profit')
plt.grid(True)
plt.show()

fig, axes = plt.subplots(1, 3, figsize=(20, 6))
category_counts.plot(kind='bar', ax=axes[0], color='skyblue')
axes[0].set_title('Transaction Count by Category')
axes[0].set_xlabel('Category')
axes[0].set_ylabel('Count')
axes[0].tick_params(axis='x', rotation=45)
sub_category_counts.plot(kind='bar', ax=axes[1], color='lightcoral')
axes[1].set_title('Transaction Count by Sub-Category')
axes[1].set_xlabel('Sub-Category')
axes[1].set_ylabel('Count')
axes[1].tick_params(axis='x', rotation=45)
region_counts.plot(kind='bar', ax=axes[2], color='lightgreen')
axes[2].set_title('Transaction Count by Region')
axes[2].set_xlabel('Region')
axes[2].set_ylabel('Count')
axes[2].tick_params(axis='x', rotation=45)
plt.tight_layout()
plt.show()

# --- Deep Dive into Unprofitability Drivers ---

# Analyze pricing and cost structures by category and sub-category
furniture_technology_high_sales_low_profit = high_sales_low_profit[
    high_sales_low_profit['Category'].isin(['Furniture', 'Technology'])]

furniture_tables_chairs_high_sales_low_profit = furniture_technology_high_sales_low_profit[
    (furniture_technology_high_sales_low_profit['Category'] == 'Furniture') &
    (furniture_technology_high_sales_low_profit['Sub-Category'].isin(['Tables', 'Chairs']))]

furniture_technology_avg = furniture_technology_high_sales_low_profit[['Sales', 'Profit', 'Discount']].mean()
furniture_tables_chairs_avg = furniture_tables_chairs_high_sales_low_profit[['Sales', 'Profit', 'Discount']].mean()

print("\nAverage Sales, Profit, and Discount for High Sales, Low/Negative Profit in Furniture and Technology categories:")
print(furniture_technology_avg)

print("\nAverage Sales, Profit, and Discount for High Sales, Low/Negative Profit in Furniture - Tables and Chairs sub-categories:")
print(furniture_tables_chairs_avg)

# Analyze regional factors
regional_high_sales_low_profit = high_sales_low_profit[
    high_sales_low_profit['Region'].isin(['Central', 'Southeast Asia', 'Oceania'])]

regional_avg = regional_high_sales_low_profit[['Sales', 'Profit', 'Shipping Cost']].mean()

print("\nAverage Sales, Profit, and Shipping Cost for High Sales, Low/Negative Profit in Central, Southeast Asia, and Oceania regions:")
print(regional_avg)

# Identify other contributing factors
order_priority_counts = high_sales_low_profit['Order Priority'].value_counts()
ship_mode_counts = high_sales_low_profit['Ship Mode'].value_counts()
quantity_mean_high_sales_low_profit = high_sales_low_profit['Quantity'].mean()

print("\nValue counts for Order Priority in high_sales_low_profit:")
print(order_priority_counts)
print("\nValue counts for Ship Mode in high_sales_low_profit:")
print(ship_mode_counts)
print(f"\nMean of Quantity in high_sales_low_profit: {quantity_mean_high_sales_low_profit:.4f}")


``` 

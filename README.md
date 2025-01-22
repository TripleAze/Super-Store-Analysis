# Superstore Sales and Profit Analysis

## Project Overview
This project involves analyzing sales and profit data from a Superstore dataset to uncover trends, insights, and areas for improvement. Using Python and libraries such as Pandas, Matplotlib, and Seaborn, the analysis focuses on:
- Sales and profit trends by category, region, and time.
- Identifying top-performing and underperforming products and branches.
- Understanding customer segments and shipping modes.

## Dataset Information
- **Source**: [Superstore Dataset](https://www.kaggle.com/)
- **Features**:
  - Sales, Profit, Discount, Quantity, and more.
  - Order and shipping details.
  - Customer and regional information.

## Key Steps
### 1. Data Loading and Preprocessing
```python
# Import necessary libraries
import pandas as pd
import numpy as np
from scipy import stats
from matplotlib import pyplot as plt
import seaborn as sns

# Load dataset
df = pd.read_csv('Sample - Superstore.csv', encoding='latin1')

# Data overview
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

# Convert date columns
df['Order Date'] = pd.to_datetime(df['Order Date'])
df['Ship Date'] = pd.to_datetime(df['Ship Date'])
```

### 2. Exploratory Data Analysis
#### Correlation Heatmap
```python
# Generate a correlation heatmap
correlation_matrix = df[['Sales', 'Profit', 'Discount', 'Quantity']].corr()
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Correlation Heatmap')
plt.show()
```
**Key Findings**:
- Positive correlation between Sales and Profit.
- Negative correlation between Discount and Profit.

### 3. Top-Performing Branches
#### Top 10 Branches by Total Sales
```python
# Aggregate sales by branch
branch_sales = df.groupby('City').agg(total_sales=('Sales', 'sum')).reset_index()
branch_sales['percentage_of_total_sales'] = (branch_sales['total_sales'] / branch_sales['total_sales'].sum()) * 100

# Top 10 branches by sales
top_10_branches = branch_sales.sort_values(by='total_sales', ascending=False).head(10)
sns.barplot(x='total_sales', y='City', data=top_10_branches, palette='Blues_d')
plt.title('Top 10 Branches by Total Sales')
plt.xlabel('Total Sales')
plt.ylabel('Branch')
plt.show()
```
**Insight**: New York City leads in total sales, contributing 11.2% of overall sales.

### 4. Product Line Analysis
#### Top 10 Products by Profit
```python
# Top 10 most profitable products
top_products = df.groupby('Product Name')['Profit'].sum().sort_values(ascending=False).head(10)
sns.barplot(x=top_products.values, y=top_products.index, palette='Set2')
plt.title('Top 10 Profitable Products')
plt.xlabel('Total Profit')
plt.ylabel('Product Name')
plt.show()
```
**Insight**: High profitability is concentrated in a few product lines.

### 5. Shipping Mode Analysis
#### Average Shipping Time and Profit
```python
# Calculate shipping time
df['Shipping Time (Days)'] = (df['Ship Date'] - df['Order Date']).dt.days

# Analyze shipping modes
shipping_analysis = df.groupby('Ship Mode').agg({
    'Sales': 'sum', 
    'Profit': 'sum', 
    'Shipping Time (Days)': 'mean'
}).reset_index()

# Visualize profits by shipping method
sns.barplot(data=shipping_analysis, x='Ship Mode', y='Profit', palette='Blues_d')
plt.title('Profit by Shipping Method')
plt.xlabel('Shipping Method')
plt.ylabel('Total Profit')
plt.show()
```
**Key Findings**:
- Standard Class generates the highest profit but has the longest shipping time.

### 6. Time-Series Analysis
#### Monthly Sales and Profit Trends
```python
# Aggregate monthly trends
monthly_trends = df.groupby(df['Order Date'].dt.to_period('M'))[['Sales', 'Profit']].sum()

# Plot monthly trends
monthly_trends.plot(figsize=(12, 6))
plt.title('Monthly Sales and Profit Over Time')
plt.xlabel('Month')
plt.ylabel('Total')
plt.show()
```
**Insight**: 2017 was the best-performing year with peak sales and profit.

## Insights and Recommendations
### Insights
1. **Top Branch**: New York City drives significant sales.
2. **High-Profit Products**: Products like Copiers and Labels yield strong profits.
3. **Discounts Impact**: Higher discounts reduce profitability.
4. **Shipping Modes**: Standard Class is preferred for affordability but impacts delivery time.

### Recommendations
- Focus on promoting high-profit products.
- Optimize discount strategies to maintain profitability.
- Streamline shipping processes for better customer satisfaction.

## Conclusion
This analysis highlights actionable insights to enhance profitability and operational efficiency. Explore the [full code and visuals](https://github.com/username/repo) for more details.

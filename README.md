# Business-development
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Set up Seaborn style for plots
sns.set_theme()

# 1. Generate mock data
# Create sample customers
customers = pd.DataFrame({
    'CustomerID': range(1, 101),
    'Name': ['Customer_' + str(i) for i in range(1, 101)],
    'Location': np.random.choice(['North', 'South', 'East', 'West'], 100),
    'CustomerType': np.random.choice(['Regular', 'Premium', 'VIP'], 100, p=[0.6, 0.3, 0.1])
})

# Create sample products
products = pd.DataFrame({
    'ProductID': range(1, 21),
    'ProductName': ['Product_' + str(i) for i in range(1, 21)],
    'Category': np.random.choice(['Electronics', 'Furniture', 'Clothing'], 20),
    'Price': np.random.randint(10, 100, 20)
})

# Generate sales data
sales = pd.DataFrame({
    'SaleID': range(1, 501),
    'CustomerID': np.random.choice(customers['CustomerID'], 500),
    'ProductID': np.random.choice(products['ProductID'], 500),
    'Quantity': np.random.randint(1, 5, 500),
    'Date': pd.date_range(start='2024-01-01', periods=500, freq='D')
})

# Merge data
sales = sales.merge(customers, on='CustomerID').merge(products, on='ProductID')

# Calculate total sales amount
sales['TotalAmount'] = sales['Quantity'] * sales['Price']

# 2. Analyze Business Performance
# Total revenue
total_revenue = sales['TotalAmount'].sum()
print(f"Total Revenue: ${total_revenue:,.2f}")

# Revenue by product
revenue_by_product = sales.groupby('ProductName')['TotalAmount'].sum().sort_values(ascending=False)
print("\nTop 5 Products by Revenue:")
print(revenue_by_product.head(5))

# Revenue by customer type
revenue_by_customer_type = sales.groupby('CustomerType')['TotalAmount'].sum()
print("\nRevenue by Customer Type:")
print(revenue_by_customer_type)

# 3. Visualize Insights
# Revenue by product (bar plot)
plt.figure(figsize=(10, 6))
revenue_by_product.head(10).plot(kind='bar', color='skyblue')
plt.title("Top 10 Products by Revenue")
plt.xlabel("Product Name")
plt.ylabel("Total Revenue ($)")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Revenue by location (pie chart)
revenue_by_location = sales.groupby('Location')['TotalAmount'].sum()
plt.figure(figsize=(8, 8))
revenue_by_location.plot(kind='pie', autopct='%1.1f%%', startangle=140, colors=sns.color_palette("pastel"))
plt.title("Revenue by Location")
plt.ylabel("")
plt.show()

# 4. Save analyzed data
sales.to_csv('business_sales_data.csv', index=False)
print("\nSales data has been saved as 'business_sales_data.csv'.")

# Supply-Chain-Analysis
Project Supply Chain Analysis

# Project Introduction
In today's fast-paced and data-driven retail and manufacturing landscape, understanding product performance, customer behavior, and supply chain efficiency is crucial for making informed business decisions. This analysis project explores a comprehensive dataset that captures various aspects of product sales, customer demographics, inventory, logistics, and manufacturing processes. The dataset includes key variables such as product type, SKU, pricing, availability, customer demographics, sales metrics, shipping and production details, and supplier information.

The goal of this project is to uncover actionable insights by addressing the following core questions:

**1.Which products have the highest sales volume and revenue?**
Identifying top-performing products helps in optimizing inventory, marketing focus, and restocking strategies.

**2.Is there a correlation between product pricing and sales volume?**
Understanding this relationship can support pricing strategies that maximize revenue without sacrificing sales.

**3.What is the comparison of sales quantity between Female and Male customers?**
Analyzing gender-based purchasing trends can provide direction for targeted promotions and personalized marketing.

**4.How do different shipping carriers compare based on shipping time and shipping cost?**
Analyzing carrier performance can help identify the most cost-effective and timely shipping partners, supporting operational efficiency and improved customer satisfaction.

**5.How do different suppliers compare based on Manufacturing costs,defect rates and production volume?**
Evaluating supplier performance helps in managing quality control, procurement efficiency, and overall supply chain reliability.

**6.How much does the mode of transportation affect defect rates?**
Investigating the impact of transportation modes on product quality allows for better logistics decisions and risk mitigation in the supply chain.

Through data cleaning, exploration, visualization, and statistical analysis, this project aims to provide a data-driven overview of product and supplier performance, customer behavior, and operational bottlenecks. The insights gained can help businesses improve decision-making across sales, pricing, production, and logistics.

# Importing Our Libraries 

<pre> 
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
</pre>

# Load Our Dataset
<pre>
#we used this code to display the full columns
pd.set_option('display.max_columns',25) 
</pre>

<pre>
df = pd.read_csv('supply_chain_data.csv')
df.head()
</pre>

<pre>
# We will check for any duplicated values 
duplicates = df.duplicated()

print("Total duplicate rows:", duplicates.sum())
</pre>

<pre>
# We will check for any Null values
df.isnull().sum()
</pre>

<pre>
df.info()
</pre>

# 1.Which products have the highest sales volume and revenue?
<pre>
#We will group by Product type and get the sum of Total sold products and sum of Reveune generated
sales_revenue = df.groupby('Product type').agg({'Number of products sold' : 'sum', 'Revenue generated' : 'sum'})\
.sort_values(by = 'Revenue generated', ascending=False).reset_index()

# here we used this code to round the reveune generated values to the first two digits only
sales_revenue['Revenue generated'] = sales_revenue['Revenue generated'].round(2) 
sales_revenue
</pre>

<pre>
# here we are creating an array with product type list Length to use it as an X axis in our visualization
x = np.arange(len(sales_revenue['Product type']))
#This width value is the Bar Width
width = 0.35
</pre>

<pre>
#Visualization
fig , ax1 = plt.subplots(figsize = (10,7))
#The first bar stands for the Total sold product
bar1 = ax1.bar(x - width/2, 'Number of products sold', data = sales_revenue , color = 'skyblue', width = width)
ax1.set_xlabel('Product Type')
ax1.set_ylabel('Total Sales')
ax1.tick_params(axis = 'y' , labelcolor = 'skyblue')

#here we created a dual Y axis duo to  the huge differences between Total sold product and Reveune 
ax2 = ax1.twinx()

#The second bar stands for the Reveune
bar2 = ax2.bar(x + width/2 , 'Revenue generated', data = sales_revenue, width = width, color = 'orange')
ax2.set_ylabel('Revenue')
ax2.tick_params(axis = 'y', labelcolor = 'orange')

plt.xticks(x, sales_revenue['Product type'])
plt.title('Total Sales and Revenue by Product Type')
fig.tight_layout()
plt.show()
</pre>

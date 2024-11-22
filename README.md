# Walmart Data Analysis

## Project Overview

**Project Title**: Walmart Data Analysis  `

This project is designed to demonstrate Python(Pandas) skills and techniques typically used by data analysts to explore, clean, and analyze data. The project involves cleaning, performing exploratory data analysis (EDA), and answering specific business questions through Pandas.This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in Pandas.

## Objectives

1. **Loadind Walmart sales data**: Load walmart sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use Pandas to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Data

- **Data**:Load walmart sales data.

```python
import pandas as pd
import numpy as np

df=pd.read_csv(r"C:\Users\Bhargav\Downloads\Walmart.csv")
```

### 2. Data Exploration & Cleaning

- **Top Rows**: Determine the top 5 records of the dataset.
- **dataset Shape**: Find out how many rows and columns are in the dataset.
- **Statistical Summary**: Find out the statistical summary.
- **Duplicate Value Check**: Check for any duplicate values in the dataset and delete records with duplicate data.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```Python
df.head()
df.shape
df.tail()
df.describe()
df.info()

df.duplicated().sum()
df.drop_duplicates(inplace=True)
df.duplicated().sum()

(df.isnull().sum()/df.shape[0])*100
df.dropna(inplace=True)

df.dtypes
df['unit_price']=df['unit_price'].str.replace("$", "").astype('float')
df.rename(columns={'unit_price': 'unit_price($)'}, inplace=True)

df['date_time'] = pd.to_datetime(df['date'] + ' ' + df['time'])
df.drop(['date','time'],axis=1,inplace=True)
```

### 3. Data Analysis & Findings

The following Python codes were developed to answer specific business questions:

1. **Question: What are the different payment methods, and how many transactions and items were sold with each method?**:
```Python
df['payment_method'].value_counts()

df.groupby('payment_method')['quantity'].sum()
```

2. **Question: Which category received the highest average rating in each branch?**:
```Python
avg_ratings = df.groupby(['Branch', 'category'])['rating'].mean().reset_index()
highest_avg_rating_category = avg_ratings.loc[avg_ratings.groupby('Branch')['rating'].idxmax()]
highest_avg_rating_category
```

3. **Question: What is the busiest day of the week for each branch based on transaction volume?**:
```Python
df['Day_of_Week'] = df['date_time'].dt.day_name()

transaction_volume = df.groupby(['Branch', 'Day_of_Week']).size().reset_index(name='Transaction_Volume')
busiest_day = transaction_volume.loc[transaction_volume.groupby('Branch')['Transaction_Volume'].idxmax()]
busiest_day
```

4. **Question: How many items were sold through each payment method?**:
```Python
df.groupby('payment_method')['quantity'].sum()
```

5. **Question: What are the average, minimum, and maximum ratings for each category in each city?**:
```Python
df.groupby(['City','category'])['rating'].max()
df.groupby(['City','category'])['rating'].min()
df.groupby(['City','category'])['rating'].mean()
```

6. **Question: What is the total profit for each category, ranked from highest to lowest?**:
```sql
df["Total_Price"]=df['unit_price($)']*df['quantity']
df['Profit']=df["Total_Price"]*df['profit_margin']

df.groupby('category')['Profit'].sum().sort_values(ascending=False)
```

7. **Question: What is the most frequently used payment method in each branch?**:
```Python
payment_counts = df.groupby('Branch')['payment_method'].value_counts()
top_payment_method = payment_counts.groupby('Branch').head(1).reset_index(name='count')
top_payment_method
```

8. **Question: How many transactions occur in each shift (Morning, Afternoon, Evening) across branches?**:
```Python
df['Hour'] = df['date_time'].dt.hour
def assign_shift(hour):
    if 6 <= hour < 12:
        return 'Morning'
    elif 12 <= hour < 18:
        return 'Afternoon'
    else:
        return 'Evening'
df['Shift'] = df['Hour'].apply(assign_shift)
shift_counts = df.groupby(['Branch', 'Shift']).size().reset_index(name='Transaction_Count')
shift_counts
```

9. **Question: Top 5 Profit producing branches?**:
```Python
df.groupby(['Branch'])['Profit'].sum().sort_values(ascending=False).head(5)
```

10. **Question: Top 5 Profit producing Cities?**:
```Python
df.groupby(['City'])['Profit'].sum().sort_values(ascending=False).head(5)
```


## Findings

- **Sale Analysis**: The products distributed across different categories such as Fashion accessories,Home and lifestyle,Electronic accessories,Food and beverages,Sports and travel,Health and beauty
- **Time of transactions**: Maximum transactions occur during Afternoon
- **Customer Insights**: The analysis identifies Most customers use credit card as payment method.
- **Profit Analysis**: Fashion accessories and Home and lifestyle categories produce maximum profits.
                       WALM009,WALM030,WALM003,WALM029,WALM046 are maximum profit producing branches.
                       WALM097,WALM100,WALM092,WALM098,WALM077 are minimum profit producing branches.
                       Weslaco,Waxahachie,Plano,Richardson,San Antonio are maximum profit producing Cities.
                       Alice,Canyon,Lake Jackson,Mineral Wells,Coppell are maximum profit producing Cities.


## Conclusion

This project serves as a basic introduction to Pandas for data analysts, covering data cleaning, exploratory data analysis, and business-driven Pandas queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.


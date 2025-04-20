# Northwind-Traders-Analysis


## Table of contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data cleaning/Preparation](#data-cleaning-preparation)
- [Exploratory data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings](#findings)
- [Recommendations](#recommendations)

### Project Overview

Northwind Traders is experiencing a decline in profitability in recent quarters. Management believes that inefficiencies in product sales performance, customer management, and regional marketing 
strategies are key contributors. This project focuses on analyzing sales trends, customer behaviors, and operational bottlenecks using the company’s historical data and provide actionable recommendations 
to boost profitability.

### Data Sources

The primary dataset used for this analysis is a comprehensive collection of sales, product, shipping, and customer information from the Sample Superstore. 
This dataset includes detailed records on transactions, product categories, shipping methods, customer demographics, and regional sales performance. 

### Tools

- **SQL**: Used for data retrieval and data manipulation.
    - [Download here](www.microsoft.com)
- **Python**: Used to clean and preprocess the extracted data for deeper analysis.
- **Power BI**: Utilized for comprehensive data analysis and visualization, enabling the creation of interactive and insightful dashboards that drive business decisions.

### Data cleaning/Preparation

Data cleaning and preparation are crucial steps to ensure the accuracy and reliability of the analysis. The following tasks were performed at initial stage;

1. **Removing Duplicates**: Identifying and eliminating duplicate records to avoid redundancy and ensure data integrity.
2. **Handling Missing Values**: Addressing missing or incomplete data by using techniques such as imputation, interpolation, or removal, depending on the context and importance of the data.
3. **Standardizing Data Formats**: Ensuring consistency in data formats, such as dates, currency, and categorical variables, to facilitate accurate analysis.
4. **Correcting Errors**: Identifying and correcting any inaccuracies or inconsistencies in the dataset, such as typos, incorrect values, or misclassified entries.
5. **Filtering and Validating Data**: Filtering out irrelevant or outlier data points that could skew the results, and validating the accuracy and reliability of the remaining data.

By thoroughly cleaning and preparing the data, we can ensure that the resulting analysis and insights are based on high-quality, accurate information.

### Exploratory data Analysis

- What is the overall sales trend over the given time period?
- Which periods have the highest sales ?
- How does the sales performance vary by product category?
- How can customers be segmented based on their purchasing behavior and demographics?
- What are the shipping costs and efficiency for different shipping methods?

### Data Analysis

- **Top 20% of customers driving the most revenue**
``` sql
with revenuepercustomer as (
select c.customerid, c.companyname, count(od.orderid) AS `total_number_of_orders_by_customer`,
sum(od.unitprice * od.quantity) as `total_revenue_generated_by_customer`
from `order details` as od
join orders as o on o.orderid = od.orderid
join customers as c on c.CustomerID = o.CustomerID
group by c.customerid
order by `total_revenue_generated_by_customer` desc
),

cumulativerev as (
select customerid, CompanyName, `total_revenue_generated_by_customer`,
sum(`total_revenue_generated_by_customer`) over (order by `total_revenue_generated_by_customer` desc) as cumulativerevenue,
sum(`total_revenue_generated_by_customer`) over() as Totaloverallrevenue,
row_number() over (order by `total_revenue_generated_by_customer` desc) as ranking 
from revenuepercustomer
)

select customerid, companyname, `total_revenue_generated_by_customer`, cumulativerevenue,
round((cumulativerevenue / Totaloverallrevenue) * 100, 2) as cumulativepercentage, ranking
from cumulativerev
order by `total_revenue_generated_by_customer` desc;
```

### Findings
Here's a summary of the analysis results:
- **Customer Revenue Contribution**: Three customers contribute 25.57% of the total revenue, posing a significant risk of revenue and profit loss if any of these top customers are lost.
- **Customer Segmentation**: There are 86 customers categorized as average spenders and no-loyalty customers. Also, 4 customers never made an order.
- **Top Product Categories**: The top five product categories are Beverages, Dairy Products, Meat/Poultry, Confections, and Seafood. The top products are  Cte de Blaye, Thringer Rostbratwurst, Raclette Courdavault, Camembert Pierrot, Tarte au sucre, 
- **Least Product by Revenue**: These products are chocolade, geitost, Genen shouyu, Laughing Lumberjack Lager and Longlife Tofu.
- **Top Region sales**: The top regional performance is the eastern region contributing approximately 52% of Total revenue.
- **Freight Costs per year** : The average freight per order in 1998 was ₦82.2, the highest recorded over the years.
- **Shipping Companies' Freight Costs**: United Package and Federal Shipping have average freight costs per order that exceed the total average freight.Federal Shipping has a high average freight cost despite
  having a low total number of orders, indicating inefficiency and high costs per order.


### Recommendations
- **Customer Retention**: Develop strategies to retain the top three customers to mitigate the risk of significant revenue loss. Personalized engagement, loyalty programs, and exclusive offers could help in maintaining these key relationships.
- **Loyalty Programs**: Implement loyalty programs aimed at converting average spenders and no-loyalty customers into loyal, repeat buyers. This could include rewards for frequent purchases, personalized discounts, and targeted marketing campaigns.
- **Product Category Focus**: Focus on optimizing the supply chain and marketing efforts for the top five product categories to maximize profitability. Ensuring these categories are well-stocked and promoted can drive higher sales. Evaluate the viability of low-performing products and consider discontinuing or rebranding them.
- **Freight Cost Analysis**: Investigate the reasons behind the high freight costs in 1998 and implement strategies to reduce these expenses. This might involve renegotiating shipping rates, improving logistics efficiency, or exploring alternative shipping methods.
- **Shipping Company Evaluation**: Evaluate the performance and cost-effectiveness of United Package and Federal Shipping. Consider renegotiating freight rates or switching to more cost-effective shipping options if necessary to lower overall freight costs.
- **Regional sales**: Enhance marketing efforts and customer engagement in the Eastern Region to leverage its strong performance. Explore opportunities to replicate successful strategies in other regions.



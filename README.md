## DSA_KMS_Inventory
This is my second practice project under the DSA program. Here, I will be working with a hypothetical business outfit, Kultra Mega Stores (KMS).

Kultra Mega Stores (KMS), headquartered in Lagos, specialises in ofﬁce supplies and furniture. I have been engaged as a Business Intelligence Analyst to support the Abuja division of KMS and provide my key insights and ﬁndings based on the excel dataset received. 

My tool for analysis is MS SQL server.
### Project Topic
Kultra Mega Stores Inventory
### Project Overview
Kultra Mega Stores (KMS), headquartered in Lagos, specialises in ofﬁce supplies and furniture. Its customer base includes individual consumers, small businesses (retail), and large corporate clients (wholesale) across Lagos, Nigeria. 

I have been engaged as a Business Intelligence Analyst to support the Abuja division of KMS. The Business Manager has shared an Excel ﬁle containing order data from 2009 to 2012 and has requested that I analyze the data and present my key insights and ﬁndings.
### Data Source
Two datasets were provided for this project- Order_status [download here](https://drive.google.com/file/d/1Tq8ccM4LoX50BPxkpfXaaCrlXFx5P_6Y/view?usp=sharing) and KMS Sql Case Study [download here](https://drive.google.com/file/d/1X9In-lAhoTUIC4WcVwgtKcCFesi7O2Rg/view?usp=sharing)

These files contain information on things like **customer name**, **customer segment**,  **sales**, **profit**, etc., all collected between the years 2009 and 2012, and which will prove useful for our analysis.
### Tools Used
- Excel for data collection
- Microsoft word (for documenting steps and findings)
- SQL server (for querying and analysis)
- Microsoft PowerPoint(for presentation of findings)

### Data Cleaning and Preparation
- Database creation: A new database called KMS_db was first created on my RDBMS, MS SQL Server. 
- Data loading and inspection: The CSV files for order_status and KMS sql case study were imported into the RDBMS. The file KMS sql case study was renamed **Order** and will from hereon out be referred to as the Order table.  
- Updating the data type: After importing the files, updates had to be made for specific columns with decimal values on the KMS sql case study dataset. These columns include: **sales**, **discount**, **profit**, **unit _price**, **shipping_cost**, and **product_base_margin**. The purpose of this was to change them to values with 10 whole numbers and three decimal places. Here is an example of the query run for **Sales**.
```SQL
ALTER TABLE [order] 
ALTER COLUMN sales DECIMAL (10,3)
```
### Exploratory Data analysis (EDA)
This involves exploring the dataset provided to provide insights. The following are the areas where KMS requires insights to be provided:
- Which product category had the highest sales?
- What are the Top 3 and Bottom 3 regions in terms of sales?
- What were the total sales of appliances in Ontario?
- Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers
- KMS incurred the most shipping cost using which shipping method?
- Who are the most valuable customers, and what products or services do they typically purchase?
- Which small business customer had the highest sales?
- Which Corporate Customer placed the most number of orders in 2009 – 2012?
- Which consumer customer was the most proﬁtable one?
- Which customer returned items, and what segment do they belong to?
- If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer
### Findings From the Order Dataset
- There are repeated product ids for different products shipped to the same customers on the same day. This means that products to be delivered to any customer on the same day are issued the same product id.
- Some customers are returning customers (i.e., they names appear on our dataset more than once). We want to take note of this when trying to compute things like most valuable customers, top 3 or bottom 3 customers, etc.
### Data Analysis and Visualization
This is where I have included the queries I ran and the results arrived at that will inform the insights that will be presented to the management at Kultra Mega stores.
#### ***1. Product category with the highest sales***
```SQL
SELECT TOP 1 product_category, SUM(sales) AS [Total Sales]
FROM [order]
GROUP BY product_category
ORDER BY [Total Sales] DESC
```
Our analysis reveals that the product category with the highest sales is **Technology** with **5984248.409**

![image](https://github.com/user-attachments/assets/2814bf32-460c-4ebb-b4f1-c93f2619b877)

![image](https://github.com/user-attachments/assets/9354675b-849b-4ee4-a51a-0e1e5decdc6c)
#### ***2a. Top 3 regions in terms of sales***
```SQL
SELECT TOP 3 Region, SUM(sales) AS [Total Sales]
FROM [order]
GROUP BY Region
ORDER BY [Total Sales] DESC
```
The top 3 regions in terms of sales are the **West** with **3597549.329**, **Ontario** with **3063212.527** and **Prarie** with **2837304.650**

![image](https://github.com/user-attachments/assets/e7fbc9e4-6fb0-490d-b8d9-8bd7fd3d6f01)

![image](https://github.com/user-attachments/assets/65f3f33f-e0cf-4649-bdf6-824c1fd4b838)
#### ***2b. Bottom 3 regions in terms of sales***
```SQL
SELECT TOP 3 Region, SUM(sales) AS [Total Sales]
FROM [order]
GROUP BY Region
ORDER BY [Total Sales] ASC
```
The bottom 3 regions in terms of sales are **Nunavut** with **116376.486**, **Northwest Territories** with **800847.341**, and **Yukon** with **975867.383**

![image](https://github.com/user-attachments/assets/f1dad561-11c6-458d-b809-8af5bacc98e8)

![image](https://github.com/user-attachments/assets/881cd1f9-038e-4535-a0df-a560edebe9af)
#### ***3. Total sales of appliances in Ontario***
```SQL
SELECT product_sub_category, SUM(sales) AS [Total Sales]
FROM [order] AS [Total Appliances sold in Ontario]
WHERE region = 'Ontario' AND product_sub_category = 'appliances'
GROUP BY product_sub_category
ORDER BY [Total Sales] DESC
```
The total sales of appliances in **Ontario** is **202346.840**

![image](https://github.com/user-attachments/assets/a812744b-91b8-4e11-a897-bbfd54d1a99a)

![image](https://github.com/user-attachments/assets/41d27cde-51f4-4281-ab9f-a907c3eafb7b)
#### ***4. Bottom 10 customers***
```SQL
SELECT TOP 10 customer_name, customer_segment, Product_Category, SUM (sales) AS [Total Sales per Customer]
FROM (SELECT customer_name, customer_segment, product_category, product_sub_category, sales, profit FROM [Order ]) 
AS [Bottom 10 customers] 
GROUP BY customer_name, Customer_Segment, Product_Category
ORDER BY [Total Sales per Customer] ASC
```
![image](https://github.com/user-attachments/assets/c0557cf3-fdb3-44f7-842e-fb46d119a12a)
#### ***5. Most expensive shipping method***
```SQL
SELECT ship_mode, SUM (shipping_cost) AS [Total Shipping Cost]
FROM [Order ]
GROUP BY ship_mode
ORDER BY [Total Shipping Cost] DESC
```
KMS incurred the most shipping cost of **51971.940** using **Delivery Trucks**

![image](https://github.com/user-attachments/assets/8428d3be-f5a1-4cae-958b-f8012d02cfa8)

![image](https://github.com/user-attachments/assets/8e32e2bf-4c12-4de2-bd32-05edcbf1afc8)
#### ***6. The most valuable customers and their purchase patterns***
```SQL
SELECT TOP 5 customer_name, product_category,SUM (sales) AS [Total Sales per Customer]
FROM (SELECT customer_name, customer_segment, product_category, product_sub_category, sales FROM [Order ]) AS [Most Valuable Customers] 
GROUP BY customer_name, Product_Category
ORDER BY [Total Sales per Customer] DESC
```
The result below shows KMS' five most valuable customers.

![image](https://github.com/user-attachments/assets/23ce4e18-7a59-40de-9d24-337876c685cd)
#### ***7. The small business customer with the highest sales***
```SQL
SELECT TOP 1 customer_name, customer_segment, SUM (sales) AS [Total Sales]
FROM (select customer_name, customer_segment, sales FROM [Order ]) AS [Small Business Customer with the Highest Sales] 
WHERE customer_segment = 'Small Business'
GROUP BY customer_name, customer_segment
ORDER BY [Total Sales] DESC
```
The small business customer with the highest sales is **Dennis Kane** with **75967.591**

![image](https://github.com/user-attachments/assets/b6cb120c-e4d7-4698-9b6f-c5b19161ffc8)

![image](https://github.com/user-attachments/assets/a1af4114-b550-43e8-af00-71faa2b29516)
#### ***8.  Corporate Customer who placed the most number of orders between 2009 and 2012***
```SQL
SELECT customer_name, SUM (order_quantity) AS [Total Order Quantity]
FROM (SELECT customer_name, customer_segment, product_category, product_sub_category, order_date, order_quantity FROM [Order ]) AS [Biggest Corporate Customer] 
WHERE customer_segment = 'corporate'
GROUP BY customer_name
ORDER BY [Total Order Quantity] DESC
```
**Roy Skaria** ranks the highest with **773** orders placed between 2009 and 2012

![image](https://github.com/user-attachments/assets/5b692c39-6898-4452-85b0-253a9d7c449a)

![image](https://github.com/user-attachments/assets/bf99c069-e49e-4790-921d-fb3b3ded8777)
#### ***9.   Most profitable consumer customer***
```SQL
SELECT TOP 1 customer_name, customer_segment, SUM (Profit) AS [Total Profit per Customer]
FROM (SELECT customer_name, customer_segment, product_category, product_sub_category, profit FROM [Order ]) AS [Most Profitable Consumer Customer] 
WHERE Customer_Segment = 'Consumer'
GROUP BY customer_name, customer_segment
ORDER BY [Total Profit per Customer] DESC
```
The most profitable consumer customer is **Emily Phan** with profit of **34005.440**

![image](https://github.com/user-attachments/assets/1946b50a-5a1b-434e-91ae-2cfc1abff6e5)
![image](https://github.com/user-attachments/assets/08f347d6-4958-4dca-b91d-597172486412)
#### ***10. Customers with returned items and their segments***
```SQL
SELECT * FROM (SELECT customer_name, customer_segment, order_id, product_category, product_sub_category , order_date FROM [Order ]) AS ord
JOIN (SELECT order_id, [status] FROM [order status]) AS ods
ON ord.order_id = ods.order_id
```
A total of **872** items were returned
![image](https://github.com/user-attachments/assets/f2e4928e-81d5-48a6-8944-8db5fcbc3119)
#### ***11. Shipping costs based on the Order Priority***
```SQL
select  ship_mode, order_priority, sum (shipping_cost) as [Total shipping cost]
from (select ship_mode, order_priority, shipping_cost from [Order ]) as [Most Profitable Consumer Customer] 
where Ship_Mode = 'delivery truck' or Ship_Mode = 'Express air'
group by ship_mode, Order_Priority
order by [Total shipping cost] desc
```
If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, did the company appropriately spend shipping costs based on the Order Priority? To determine when a company appropriately spends shipping costs based on order priority, we can categorize the shipping methods according to the urgency of the orders. 

***Shipping Methods***
- Delivery Truck
  - Cost: Most economical
  - Speed: Slowest
- Express Air
  - Cost: Most expensive
  - Speed: Fastest

***Order Priorities***
- Critical: Requires immediate delivery.
- High: Needs prompt delivery but not as urgent as critical.
- Medium: Standard delivery timeframe is acceptable.
- Low: Can wait for delivery without any rush.

***Appropriate Shipping Costs Based on Order Priority***
  
| Order Priority | Recommended Shipping Method |  Justification |
| ------------- | ------------- | ------------- |
| Critical	Express  | Air | Immediate delivery is essential to meet urgent needs |
| High  | Express Air or Delivery Truck	Depending on the specific urgency and availability of budget  | Express Air if budget allows; otherwise, Delivery Truck if acceptable |
| Medium  | Delivery Truck  | Cost-effective option is suitable for standard delivery times |
| Low  | Delivery Truck  | Most economical choice is appropriate for non-urgent deliveries |

A look at the results generated below shows that the company is incurring higher shipping costs for delivery trucks over Express Air across all order priorities. This indicates a misalignment with the principles of cost-effectiveness, order priority, and customer satisfaction. Therefore, we cannot say that the comapany appropriately spent shipping costs based on the Order Priority.

![image](https://github.com/user-attachments/assets/a83cc32a-c4be-485f-84e9-8d4f81e4f67e)
### Summary of Results and Findings
- The product category with the highest sales is Technology with 5984248.409
- The top 3 regions in terms of sales are the **West** with **3597549.329**, **Ontario** with **3063212.527** and **Prarie** with **2837304.650** and the bottom 3 regions in terms of sales are **Nunavut** with **116376.486**, **Northwest Territories** with **800847.341**, and **Yukon** with **975867.383**
- KMS incurred the most shipping cost of **51971.940** using **Delivery Trucks**
- The small business customer with the highest sales is **Dennis Kane** with **75967.591**
- **Roy Skaria** ranks the highest from KMS' corporate customers with **773** orders placed between 2009 and 2012
- The most profitable consumer customer is **Emily Phan** with profit of **34005.440**
  
### Recommendations
- Product Category for focus: since technological products have brought the most revenue for KMS in the last three years, then the company is advised to really rein in on improving its production and sale of those products across its different markets.
- Regional Sales: The top 3 regions in terms of sales are the **West** with **3597549.329**, **Ontario** with **3063212.527** and **Prarie** with **2837304.650** and the bottom 3 regions in terms of sales are **Nunavut** with **116376.486**, **Northwest Territories** with **800847.341**, and **Yukon** with **975867.383**. The company is advised to channel most of its focus to making available its products in its biggest markets; the West, Ontario, and Prarie.
- The most valuable customers typically purchase technological products. And since our analysis has already revealed that technological products have brought KMS the most benefit, then this is another pointer to the fact that the company needs to prioritize the production and sale of its technological products across its different markets.
- Loyalty programs can be implemented for the business' most valuable and profitable customers. Customers may perceive greater value in their purchases when they know they are earning rewards, leading to increased spending. Loyalty programs foster a sense of connection between customers and brands, leading to increased trust and satisfaction. Customers who feel rewarded and recognized are more likely to develop a preference for a brand over competitors, leading to higher retention rates.
- From our results, furniture and office supplies have not sold well mainly to consumers, home offices, and small businesses. Customers in these segments also represent KMS' bottom customers in terms of sales. The company may implement some or all of these strategies:
  - Conduct Surveys: Gather feedback from these customers to understand their specific needs, preferences, and pain points regarding furniture and office supplies.
  - Tailored Promotions: Create personalized offers based on their purchase history or preferences. For example, if a customer frequently buys ergonomic chairs, offer discounts on related products.
  - Create Bundles and seasonal promotions: Offer bundles that combine complementary products (e.g., a desk with an office chair and supplies) at a discounted rate to encourage larger purchases. Introduce seasonal bundles that cater to specific needs, such as back-to-school or home office setups.
  - Enhanced Customer Support: Offer dedicated support for these segments, ensuring they receive quick responses to inquiries and assistance with product selection.
  - Leverage social proof using Customer Reviews and Testimonials and case studies: Highlight positive reviews and testimonials from similar customers to build trust and encourage purchases. Share success stories of how other small businesses or home offices have benefited from your products.
- Shipping Costs:
  - Cost-Effectiveness: Delivery Truck is typically the most economical option. If the company is paying more for delivery trucks than for Express Air, it indicates a misallocation of resources. The expectation is that the least expensive option should be used for lower-priority orders.
  - Order Priority Alignment: Critical and High Priority Orders should prioritize speed over cost. If the company is using delivery trucks (which are slower) and incurring higher costs, it contradicts the need for timely delivery for these orders. This suggests a failure to align shipping methods with the urgency of the orders.
  - Inefficient Resource Management: Spending more on a slower shipping method indicates poor decision-making. The company should assess shipping needs based on urgency and choose the method that balances cost with delivery speed. Higher costs for delivery trucks suggest inefficiency.
  - Customer Satisfaction: If critical orders are delayed due to the use of delivery trucks, it can lead to customer dissatisfaction. Meeting customer expectations is crucial, especially for critical and high-priority orders, which require timely fulfillment.
  - Financial Implications: Higher shipping costs for delivery trucks could negatively impact the company's profitability. It is essential to optimize shipping strategies to maintain a healthy bottom line while meeting customer demands.

# Customer-Segmentation-RFM-Analysis

## Introduction

RFM Analysis is one of the most popular customer segmentation techniques to quantify customer transaction history. It considers the customers' purchasing habits including Recency- how recently the customer purchases, Frequency which is how often the customer purchases and Monetary value, which is the value of purchases.  RFM classifies customers based on their respective scores.
Some of its benefits include:
-	Selecting customers for target mailing to reduce costs and improve sales.
-	Identifying customers to better promote and market your products and services.
-	Identify and Retaining At-risk customers.
-	Drive Engagement
  
## Problem Statement
Ratafar Mall aims to classify its customers into four segments based on their purchasing habits. This will enable them to identify valued customers, and those needing immediate attention, and create targeted marketing campaigns tailored to each group, promoting their products more effectively.

## Steps Involved in the Analysis
This project employs the use of Microsoft Power BI to clean, transform, Analyse and Visualise the data. The steps taken to achieve these are described below:

### Data Cleaning 

The data contains 11 columns and 400 rows. The columns are Customer ID, Customer Name, Purchase Date, Amount, Birth, Gender, Job title, Education, Income, Total Orders, and Picture URL. There are 25 duplicated rows which were removed from the data.

![](https://github.com/Ratafar22/Customer-Segmentation-RFM-Analysis/blob/main/Pictures/DataPreview.JPG)

### Data Transformation

Two new columns were created to calculate the customers' age and the Number of Days since the Last Order.
```sql
DateSinceLastOrder = 
DATEDIFF(
    [DateOfPurchase], 
    TODAY(),
    DAY
)
```
### Percentrank the customers

The recency value, frequency value and monetary value columns were created and ranked between 1 and 5. This enables us to have the same base value for each metric. 
The formulas used to create these columns are as follows:
```sql
Recency = 
SWITCH(
    TRUE(),
    Customer_Data[DateSinceLastOrder] <= PERCENTILE.INC(Customer_Data[DateSinceLastOrder],0.2),"5",
    Customer_Data[DateSinceLastOrder] <= PERCENTILE.INC(Customer_Data[DateSinceLastOrder],0.4),"4",
    Customer_Data[DateSinceLastOrder] <= PERCENTILE.INC(Customer_Data[DateSinceLastOrder],0.6),"3",
    Customer_Data[DateSinceLastOrder] <= PERCENTILE.INC(Customer_Data[DateSinceLastOrder],0.8),"2",
    "1"
)
```
```sql
Frequency = 
SWITCH(
    TRUE(),
    Customer_Data[TotalOrders] <= PERCENTILE.INC(Customer_Data[TotalOrders], 0.2), "1",
    Customer_Data[TotalOrders] <= PERCENTILE.INC(Customer_Data[TotalOrders], 0.4), "2",
    Customer_Data[TotalOrders] <= PERCENTILE.INC(Customer_Data[TotalOrders], 0.6), "3",
    Customer_Data[TotalOrders] <= PERCENTILE.INC(Customer_Data[TotalOrders], 0.8), "4",
    "5")
```
```sql
Monetary = 
SWITCH(
    TRUE(),
    Customer_Data[Amount] <= PERCENTILE.INC(Customer_Data[Amount], 0.2), "1",
    Customer_Data[Amount] <= PERCENTILE.INC(Customer_Data[Amount], 0.4), "2",
    Customer_Data[Amount] <= PERCENTILE.INC(Customer_Data[Amount], 0.6), "3",
    Customer_Data[Amount] <= PERCENTILE.INC(Customer_Data[Amount], 0.8), "4",
    "5")
```
### Percentrank the RFM Score

Each customer's RFM value was added together and ranked between the values of 1 and 5 to calculate the RFM Score of each customer. The table below shows the preview of the newly calculated columns.

![](https://github.com/Ratafar22/Customer-Segmentation-RFM-Analysis/blob/main/Pictures/CalculatedColumns.JPG)

### Segments 
A table containing the RFM score and Segment was created. This will enable us to know the segment each customer belongs to based on their RFM score.

![](https://github.com/Ratafar22/Customer-Segmentation-RFM-Analysis/blob/main/Pictures/SegmentTable.JPG)

### Analyze and Visualize the Data

After calculating the customers' RFM scores, it was pertinent to analyse and visualise the data to understand the distribution of customers according to the segments.

## Insight
Based on the analysis performed on 375 customers, some of the insights derived are as follows:
-	16% of customers are champions.
-	39% were classified as loyal customers.
-	20% were identified as at-risk customers.
-	25% of the customers fell into the immediate attention category.
-	Top customers (champions), On average made 8 purchases, their average spending was $2000, and their last purchase occurred, on average 112 days ago. 

- Potential Loyalists have made an average of 6 purchases while spending $1700 per purchase and their last purchase occurred an average of 196 days ago.

- The combination of At-risk and Immediate Attention customer segments purchased 4 times on average, spending $1,504. These customers' recent purchases took place on average every 268 days.

## Recommendation

**Top Customers**

-	Offer Incentives and Discounts:
Exclusive offers should be given to these customers such as discounts on products, loyalty cards, cashback schemes, and free coupon points.

-	Feature a customer of the week or month: 
_Recognizing and celebrating loyal customers will enable them to feel loved and encourage them to keep patronising your business. This is also a good way to attract other customers who might be interested in patronising the business._

-	Surprise and delight them:
_Send them personalised birthday messages with a special deal, such as upgrading their orders or giving them a bonus product or service that exceeds their expectations._

-	Reward them with an early access program:
_Consider giving them access to a new product that hasn’t officially been launched to anyone else or services that aren’t opened to the public yet._

**At Risk/Immediate Attention Customers**

-	Feedback Survey:
_A feedback survey should be carried out to help understand the causes of setbacks and ask how you can improve your services to them and ensure resolve any concerns raised by the customers._

-	Launch Re-Engagement Campaign:
Reach out to them with customized emails or messages containing unique offers or discounts to encourage them to make purchases.

-	Win-Back Incentives: _Extend appealing win-back offers to customers who haven't made purchases recently. These could include discounts, free shipping, or bundled deals to entice their return._



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



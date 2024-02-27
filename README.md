# SUPERSTORE SALES ANALYSIS

_Unlocking Superstore Insights: Exploring Data in Bigquery with SQL_

In the world of business, data-driven decisions are essential to success(rephrase this). Sales figure provide important insights to the performance of a company's products and regions. And one of the powerful data analysis tools that can be used to get insights from datasets is **SQL**.

![](stationerystore.jpeg)
                                        _stationery store_


##  PROJECT OVERVIEW

In this project, I utilized Excel, SQL, and Power BI in the analysis of the Super store dataset from Kaggle. 
The Super store is located in the United State where  the stationeries are being sold. This Superstore data is hosted on Google BigQuery for the analysis. 
The purpose of this analysis is to understand and answer some business questions regarding the performance of the Superstore, gain insights, and provide some recommendations to increase revenue.
At the time of analysis, the Superstore recorded sales transaction data from 2014 – 2017. The entire data can be accessed here [superstore dataset](https://www.kaggle.com/datasets/ishanshrivastava28/superstore-sales)

### Data Cleaning Process

The first step I took in the data analysis process was to import the Superstore dataset into Excel for data cleaning to check for errors and inconsistencies. The Superstore dataset comprises of information such as; Sales, Customer Name, Order date, Delivery date, Product category, Ship mode, Country, Region, Product Subcategory, Product Segment, Postal code.

I followed the following steps for the Data cleaning process;

- Data Importation: To kick start with this analysis, I firstly imported the dataset from the File ribbon into the excel spreadsheet.

![](DataImport.JPG)

- Data Cleaning:  To check the dataset for duplicates, I highlighted the whole columns from A to V, then clicked on the Data ribbon on remove duplicates but no duplicates was found.

![](Duplicate0.JPG)

Then I proceeded to filter each column to check for blanks, errors, misspellings or any outliers for further data cleaning. 

I discovered inconsistencies in the Order Date and Ship Date columns, I proceeded by coverting the Date value to Date with the Text to column tool in the Data ribbon as follows;


  Old Order date column     				                    |     					New Order Date column
:-------------------------------------------------------|:---------------------------------------------------------:
![](Date1.JPG)						                              |	![](Date2.JPG)


- Data Transformation: In order to calculate the Delivery day, a new column was inserted and the DATEDIF function was used to find the difference between the Ship date and Order date.


 Formula                      				                  |     				Result
:-------------------------------------------------------|:---------------------------------------------------------:
![](DelDay1.JPG)						                            |	![](DelDay2.JPG)


## BUSINESS QUESTIONS
  
In order to further analyze the data to gain insights and find trends, the dataset was hosted on Bigquery to further find the KPIs for the Superstore to know its performance.
  
1. _**Total Revenue Generated**_
 
 The SUM function was used in calculating the total sales as shown in the image below;

```sql
SELECT  sum(Sales)
FROM `coastal-stone-366510.sales.superstore`
```

![](Total2.JPG)
_Total Sales_- Image by Author



2. _**Total number of order received**_

  The COUNT function was used in calculating the total number of order received as shown in the image below;

```sql
SELECT  count(Order_ID) AS Num_of_Order
FROM `coastal-stone-366510.sales.superstore`
```
![](Order2.JPG)
 _Total Order_- Image by Author



3. _**Total Quantities Sold**_

The CAST function was used to convert the integer to FLOAT64 and the SUM was used to calculate the total quantity sold as shown in the image below;

```sql
SELECT CAST(SUM(Quantity) AS FLOAT64) as Total_Quantities
FROM `coastal-stone-366510.sales.superstore`
```
![](TQty.JPG)
_Total Quantities Sold_- Image by Author


4. _**Total quantities sold per each Product Category**_

The CAST function was used to convert the integer to FLOAT64 and the SUM was used to find the total quantity sold. The GROUP BY was used to group the different product categories as shown in the image below;

```sql
SELECT CAST(SUM(Quantity) AS FLOAT64) as Total_Quantities, Category
FROM `coastal-stone-366510.sales.superstore`
GROUP BY Category
ORDER BY Total_Quantities desc
```
![](CQty.JPG)
 _Total Order_- Image by Author
   
5. _**Sales made per each year**_

The EXTRACT function was used get the unique year number and the SUM function was used to get yearly sales as shown in the image below;

```sql
SELECT ROUND(SUM(sales), 0) AS TotalSales, EXTRACT(YEAR FROM Order_Date) AS Order_Year
FROM coastal-stone-366510.sales.superstore
GROUP BY Order_Year
ORDER BY Order_Year;
```

![](YS2.JPG)
 _Yearly Sales_- Image by Author

6. _**Total Quantites Sold per each year**_

The EXTRACT function was used get the unique year number and the SUM and CAST functions were used to get quantities sold for each year as shown in the image below;

```sql
SELECT CAST(SUM(Quantity) AS FLOAT64) as Total_Quantities, EXTRACT(YEAR FROM Order_Date) AS Order_Year
FROM `coastal-stone-366510.sales.superstore`
GROUP BY Order_Year
ORDER BY Order_Year
```
![](YQty2.JPG)

_Yearly Quuantities Sold_- Image by Author


7. _**States with the highest and least sales**_

a. _TOP5_
 
The SUM function was used to calcualte the total sales for each product sub-category and ordered in ascending order to get the bottom5 products in sales as shown in the image below;

```sql
SELECT round(sum(Sales),1) as sales, State
FROM `coastal-stone-366510.sales.superstore` 
GROUP BY State
ORDER BY sales desc
LIMIT 5
```
![](state1.JPG)
_Top 5 States in Sales_ - Image by Author

b. _BOTTOM5_

The SUM function was used to calcualte the total sales for each product sub-category and ordered in ascending order to get the bottom5 products in sales as shown in the image below;

```sql
SELECT round(sum(Sales),1) as sales, State
FROM `coastal-stone-366510.sales.superstore` 
GROUP BY State
ORDER BY sales asc
LIMIT 5
```
![](state2.JPG)

_Least 5 States in Sales_ - Image by Author


8. _**What are the highest sold products and the least sold  products in sales?**_

_TOP5 Products in sales_  

The SUM function was used to calcualte the total sales for each product sub-category and ordered in descending order to get the top5 products in sales as shown in the image below;

```sql
SELECT round(sum(sales),1) as sales, Sub_Category 
FROM `coastal-stone-366510.sales.superstore` 
GROUP BY Sub_Category
ORDER BY sales desc
LIMIT 5
```

![](PATOP.JPG)


_BOTTOM5 Products in sales_

The SUM function was used to calcualte the total sales for each product sub-category and ordered in ascending order to get the bottom5 products in sales as shown in the image below;

```sql
SELECT round(sum(sales),1) as sales, Sub_Category 
FROM `coastal-stone-366510.sales.superstore` 
GROUP BY Sub_Category
ORDER BY sales 
LIMIT 5
```
![](PALOW.JPG)


9. _**Sales by Product categories and Segment**_

The SUM function was used to calcualte the total sales for each product segment and category as shown in the image below;

```sql
SELECT Category, Segment, round(sum(Sales),0) as TotalSales
FROM `coastal-stone-366510.sales.superstore`
GROUP BY Category,Segment
ORDER BY TotalSales desc
```
![](prod2.JPG)

   
10. _**Sales by Region**_

The SUM function was used to calcualte the total sales for each region as shown in the image below;

```sql
SELECT  round(sum(Sales),1) as sales, Region
FROM `coastal-stone-366510.sales.superstore`
GROUP BY Region
ORDER BY sales desc
```
![](reg2.JPG)


11. _**Shipmode by Customer's preference**_
 
 The COUNT function was used to calculate the number of customers as shown in the image below;

```sql
SELECT count(Customer_ID) as customer, Ship_Mode
 FROM `coastal-stone-366510.sales.superstore` 
 GROUP BY Ship_Mode
 ORDER BY customer desc
```
![](ship2.JPG)


## OBSERVATIONS
- The total revenue generated was $2,297,200
- The quantities sold was 37873 units
- The total number of order received was 9994
- The products sold in the Superstore are divided into three categories: Office supplies, Technology and Furniture.  Office supplies had the highest sales, followed by Furniture while Technology had the least sales
- Total sales increased over the years and so was the total quantities sold.
- The West region had the highest sales, followed by East, then Central while the South region had the least sales.
- The State with the highest sales was California while North Dakota had the least sales
- Their top5 products sold are Phones, Chairs, Storage, Tables and Binders while the bottom5 are Supplies, Art, Envelopes, Labels and Fasteners
- Standard class was the most preferred by the customers, followed by Second class, followed by First class and lastly Same day for delivery mode.


## RECOMMENDATIONS


## DASHBOARD

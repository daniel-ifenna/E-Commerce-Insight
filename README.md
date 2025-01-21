# E-COMMERCE INSIGHT
## Introduction
This analysis is based on e-commerce data sourced from ChatGPT. I conducted an in-depth analysis to derive valuable insights from the dataset. The goal was to answer several key business questions related to sales performance and customer behavior. The questions explored include:

- What are the top 5 products by revenue?
- How many unique customers made purchases?
- Which region has the highest number of customers?
- What does the monthly sales trend look like over the dataset's timeframe?
- Which product category is the most popular each month?
- What percentage of orders were completed, canceled, and returned?
- Is there a correlation between region and order status?
  
The following report presents the findings to these questions based on the data analysis conducted. For further reference, the raw data is provided in the accompanying Excel file.[Download Here](https://1drv.ms/x/c/fc11b36f16d1a624/Eco90l1gX41Dr2Rb5xDtz9QB5zamhuFdE8iGs-1fd94k2g?e=EOkf4d)
## Explanation of Variables
- Order.ID: Unique identifier for each order.
- Customer.ID: Unique identifier for each customer.
- Order.Date: The date on which the order was placed.
- Product.Category: The category of the product purchased.
- Product.Name: The specific name of the product.
- Quantity: The number of units purchased in the order.
- price.Per.Unit: The price per individual unit of the product.
- Region: The geographic region where the customer is located.
- Order.Status: The status of the order (e.g., completed, canceled, or returned).
- Total.Price: The total price of the order.

## Analysis Findings 
a glimpse of my code structure 
~~~{r}
sales<-read.csv("Portfolio/E-commerce/ecommerce_dataset.csv")

library(tidyverse))

#Identify the top 5 products by revenue.
top_products<-sales %>% 
  group_by(Product.Name, Product.Category) %>% 
  summarise(spt=sum(Total.Price)) %>% 
  arrange(desc(spt)) %>% 
  head(5)
View(top_products)

#How do monthly sales trends look over the dataset's timeframe?
sales$order_date1<-as.Date(sales$Order.Date, format="%m/%d/%Y")
sales$year<-format(sales$order_date1, "%Y")
sales$month<-format(sales$order_date1, "%m")
sales$year_month<-format(sales$order_date1,"%Y-%m")

monthly_sales<-sales %>% 
  group_by(year_month) %>% 
  summarize(tsr=sum(Total.Price))
# Convert 'year_month' to Date format (assuming it's in "YYYY-MM" format)
monthly_sales$year_month <- as.Date(paste0(monthly_sales$year_month,","))
str(monthly_sales)

library(ggplot2)
monthly_sales %>%
  ggplot(aes(x = year_month, y = tsr)) +
  geom_line(color = "black", alpha = 0.5) +
  labs(title = "Monthly Sales Trend", x = "Months", y = "Total Sales") +
  scale_x_date(date_labels = "%b %Y", date_breaks = "1 month") +  # Set interval to 1 month
  theme_gray()

contingency_table <- table(sales$Region, sales$Order.Status)
print(contingency_table)

chi_square_result <- chisq.test(contingency_table)

print(chi_square_result)
~~~
1. Identify the top 5 products by revenue.
   The product name grouped the data result. The highest product category is
 - Item D "BOOKS" with a total sum of $23,627.46
- Item D "Home & Kitchen" a total sum of $22, 331.79
- Item C "Books" a total sum of $22,170.15
- Item E "Electronics" a total sum of $19,499.03
- Item E "Toys" has a total sum of $19,473.10

as shown in the diagram below;



   ![](https://github.com/daniel-ifenna/E-Commerce-Insight/blob/main/images/Screenshot%202025-01-21%20130320.png)




2. How many unique customers made purchases?
   - 99 customers had their orders. status-completed.

3. Which region has the highest number of customers?
   - North and West both have the highest number of customers. A total of 74 customers in both regions
    ![](https://github.com/daniel-ifenna/E-Commerce-Insight/blob/main/images/Screenshot%202025-01-21%20131858.png)







4. How do monthly sales trends look over the dataset’s timeframe?
   ![](https://github.com/daniel-ifenna/E-Commerce-Insight/blob/main/images/Monthly%20sales.png)






5. What is the most popular product category by month?
      ![](https://github.com/daniel-ifenna/E-Commerce-Insight/blob/main/images/Screenshot%202025-01-21%20132930.png)


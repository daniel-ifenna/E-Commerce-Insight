# E-COMMERCE INSIGHT
## Introduction
This analysis is based on e-commerce data sourced from ChatGPT. I conducted an in-depth analysis to derive valuable insights from the dataset. The goal was to answer several key business questions related to sales performance and customer behavior. The questions explored include:

- What are the top 5 products by revenue?
- How many unique customers made purchases?
- Which region has the highest number of customers?
- What is the average revenue per customer?
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
Code Structure 
~~~{r}
sales<-read.csv("Portfolio/E-commerce/ecommerce_dataset.csv")
str(sales)
mean(sales$Total.Price)
library(tidyverse)
#Which product category contributes the most to total sales?
most_sales<- sales %>% 
  group_by(Product.Category) %>% 
  summarise(spt=sum(Total.Price)) %>% 
  arrange(desc(spt)) %>% 
  head (1)
View(most_sales)

#Identify the top 5 products by revenue.
top_products<-sales %>% 
  group_by(Product.Name, Product.Category) %>% 
  summarise(spt=sum(Total.Price)) %>% 
  arrange(desc(spt)) %>% 
  head(5)
View(top_products)

#How many unique customers made purchases?
Unique_purchases<-sales %>% 
  filter(Order.Status=="Completed") %>% 
  select(Customer.ID) %>% 
  summarise(Unique_purchases=n_distinct(Customer.ID))#n_distinct() function counts the unique customer id
print(Unique_purchases)    

#Which region has the highest number of customers?
highest_region<- sales %>% 
  select(Region, Customer.ID) %>% 
  group_by(Region) %>% 
  summarise(customer_count=n_distinct(Customer.ID)) %>% 
  arrange(desc(customer_count))
print(highest_region)          

#What is the average revenue per customer?
Average_revenue<-sales %>% 
  select(Customer.ID,Total.Price) %>% 
  group_by(Customer.ID) %>% 
  summarise(revenue= mean(Total.Price)) %>% 
  arrange(desc(revenue))
print(Average_revenue)  

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

#What is the most popular product category by month?
popular_products<- sales %>% 
  group_by(month, Product.Category) %>% 
  summarise(count=n()) %>%  
  group_by(month) %>% 
  arrange(month, desc(count)) %>% 
  slice(1)

#What percentage of orders are completed, canceled, and returned?
order_analysis<-sales %>% 
  group_by(Order.Status) %>% #group by order status
  summarise(count=n()) %>% #Count the number of orders for each category
  mutate(percentage=count/sum(count)*100) #Calcuate the percentage for each status
print(order_analysis)

contingency_table <- table(sales$Region, sales$Order.Status)
print(contingency_table)

chi_square_result <- chisq.test(contingency_table)

print(chi_square_result)
~~~
1. Identify the top 5 products by revenue.
   ![]()


# Customer Churn Analysis: Uncovering Insights Using SQL

![](Churn-rate.jpg)

## INTRODUCTION

Customer churn analysis is an important activity for businesses of all sizes. It entails recognizing clients who are most likely to end their association with the business and comprehending the causes of churn. In order to gain useful insights, we will perform customer churn analysis using SQL in this project. We will examine a dataset of customer interactions and behavior.
Here are the follo

## SQL code to create the dataset for the project
Here's an example SQL code to create the dataset for the project:

*-- Create the customer's table*

    CREATE TABLE customers (
       customer_id INT PRIMARY KEY,
       name VARCHAR(50),
       gender VARCHAR(10),
       age INT,
       location VARCHAR(50)
    );
    
*-- Create the interactions table*

    CREATE TABLE interactions (
       interaction_id INT PRIMARY KEY,
       customer_id INT,
       date DATE,
       interaction_type VARCHAR(20),
       interaction_channel VARCHAR(20),
       FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    );

*-- Create the purchases table*

    CREATE TABLE purchases (
       purchase_id INT PRIMARY KEY,
       customer_id INT,
       product_id INT,
       date DATE,
       price DECIMAL(10,2),
      FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    );

*-- Create the churn table*

    CREATE TABLE churn (
      customer_id INT PRIMARY KEY,
      churn_date DATE,
      FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    );

*-- Insert sample data into the tables*

    INSERT INTO customers (customer_id, name, gender, age, location)
    VALUES (1, 'John Doe', 'Male', 30, 'New York'),
       (2, 'Jane Smith', 'Female', 35, 'Los Angeles'),
       (3, 'David Johnson', 'Male', 25, 'Chicago');

    INSERT INTO interactions (interaction_id, customer_id, date, interaction_type, interaction_channel)
    VALUES (1, 1, '2023-05-01', 'Email', 'Online'),
       (2, 1, '2023-05-03', 'Phone Call', 'Offline'),
       (3, 2, '2023-05-02', 'Email', 'Online');

    INSERT INTO purchases (purchase_id, customer_id, product_id, date, price)
       VALUES (1, 1, 1001, '2023-05-04', 50.00),
       (2, 1, 1002, '2023-05-05', 75.00),
       (3, 2, 1001, '2023-05-03', 50.00);

    INSERT INTO churn (customer_id, churn_date)
       VALUES (1, '2023-06-01');


## 1. Data Exploration
Data exploration is crucial in any data analysis project, It involves examining and understanding the data to gain insights and identify patterns or trends. Here are the reasons why data exploration is important in the context of customer churn analysis:
-    Understand the dataset: Data exploration helps you familiarize yourself with the dataset's structure, contents, and quality. It allows you to verify if the data is accurate, complete, and properly formatted.

-   Identify data issues: By exploring the data, you can detect any missing values, outliers, or inconsistencies that may affect the accuracy and reliability of your analysis. Addressing these issues ensures the integrity of your results.

-   Overview of customer base: Data exploration provides an overview of the customer base by determining the total number of customers. It helps understand the scale and scope of the dataset and sets the context for subsequent analysis.

## **Here are the data exploration I carried out in this PROJECT**

*Retrieve the total number of customers in the dataset.*

This task provides an overview of the dataset by determining the total count of unique customers. It helps to understand the scale of the customer base.

    SELECT COUNT(*) AS total_customers 
      FROM customers;
*Calculate the average age of customers*

By calculating the average age, I can gain insights into the age demographics of the customer base. It can help identify potential age-related patterns or preferences.      

      SELECT AVG(age) AS average_age 
        FROM customers;
*Determine the total number of interactions in the dataset.*

This task provides an understanding of the overall customer engagement by determining the total count of interactions. It helps to gauge the level of customer activity.

     SELECT COUNT(*) AS total_interactions 
       FROM interactions;
*Find the most common interaction channel used by customers.*

Identifying the most common interaction channel helps businesses understand the preferred communication channels of their customers. It can guide resource allocation and communication strategies.

     SELECT interaction_channel, COUNT(*) AS channel_count 
         FROM interactions 
         GROUP BY interaction_channel 
         ORDER BY channel_count DESC 
         LIMIT 1;

## 2. Churn Analysis
*Calculate the churn rate for the given time period.*

Churn rate measures the percentage of customers who have discontinued their relationship with the company. It is an essential metric to evaluate customer retention and loyalty.

     SELECT (COUNT(DISTINCT customer_id) * 100.0) / (SELECT COUNT(*) FROM customers) 
       AS churn_rate
       FROM churn;
*Identify the number of customers who churned.*

Determining the count of churned customers provides insights into the magnitude of customer churn and helps assess the impact on the business.

     SELECT COUNT(DISTINCT customer_id) AS churned_customers 
      FROM churn;
* Determine the average churn time for customers who churned.*

Average churn time measures the average duration between the first interaction and churn date for customers who have churned. 
It helps understand the typical customer lifespan.

    SELECT AVG(DATEDIFF(churn_date, (SELECT MIN(date) 
      FROM interactions WHERE customer_id = churn.customer_id))) 
      AS average_churn_time FROM churn;
 *Find the percentage of churned customers by gender.*

Analyzing churn rates by gender provides insights into potential gender-based variations in customer retention.
It helps identify any gender-specific patterns that may impact churn.

    SELECT gender, (COUNT(DISTINCT customer_id) * 100.0) / (SELECT COUNT(*) FROM churn) AS churn_percentage 
      FROM churn 
      GROUP BY gender; 
*Calculate the average age of churned customers.*

Analyzing the average age of churned customers helps identify if there are any age-related factors contributing to customer churn.
It helps assess the impact of age on churn behavior.
  
    SELECT AVG(age) AS average_age 
       FROM churn 
       INNER JOIN customers ON churn.customer_id = customers.customer_id;        
## 3. Purchase Analysis

*Determine the total revenue generated from purchases.*

Calculating the total revenue provides an overview of the financial impact of customer purchases. It helps evaluate the overall sales performance.

    SELECT SUM(price) AS total_revenue 
     FROM purchases;
*Calculate the average price of products purchased.*

Analyzing the average price of products helps understand customer spending patterns and price preferences. It can guide pricing strategies and product offerings.

    SELECT AVG(price) AS average_price 
    FROM purchases;
*Identify the top 5 products with the highest revenue.*

Identifying the top-selling products based on revenue helps focus on the most profitable products. It can guide inventory management and marketing strategies.

    SELECT product_id, SUM(price) AS revenue 
      FROM purchases GROUP BY product_id 
      ORDER BY revenue DESC 
      LIMIT 5; 
*Determine the average number of purchases per customer.*

Analyzing the average number of purchases per customer provides insights into customer buying behavior. It helps assess customer loyalty and engagement.

    SELECT COUNT(*) / COUNT(DISTINCT customer_id) AS average_purchases_per_customer
      FROM purchases;
*Find the average purchase price by gender.*

Analyzing the average purchase price by gender helps identify any gender-based variations in spending patterns.
It can guide marketing efforts and product targeting.

    SELECT gender, AVG(price) AS average_purchase_price 
      FROM purchases 
      INNER JOIN customers ON purchases.customer_id = customers.customer_id GROUP BY gender;

## 4. Customer Interaction Analysis

*Determine the number of interactions per customer.*

Analyzing the number of interactions per customer helps understand customer engagement levels. It can highlight highly active or disengaged customers.

     SELECT customer_id, COUNT(*) AS interaction_count 
       FROM interactions GROUP BY customer_id;



d. SELECT interaction_channel, COUNT(*) AS channel_count FROM interactions GROUP BY interaction_channel;

 *Identify the most common interaction type.*

Determining the most common interaction type provides insights into the preferred mode of customer engagement. 
It can guide communication strategies and resource allocation.

    SELECT interaction_type, COUNT(*) AS type_count 
      FROM interactions 
      GROUP BY interaction_type
      ORDER BY type_count DESC 
      LIMIT 1;
*Calculate the average number of interactions per customer by gender.*

Analyzing the average number of interactions per customer by gender helps identify any gender-based variations in customer engagement. 
It can guide personalized communication strategies.

     SELECT gender, AVG(interaction_count) AS average_interactions 
       FROM (SELECT customer_id, COUNT(*) AS interaction_count 
       FROM interactions GROUP BY customer_id) AS subquery 
       INNER JOIN customers ON subquery.customer_id = customers.customer_id GROUP BY gender;
*Find the number of interactions per interaction channel.*

Analyzing the number of interactions per interaction channel helps understand the distribution of customer interactions across different communication channels. It can guide channel-specific strategies.
      
    SELECT interaction_channel, COUNT(*) AS channel_count 
      FROM interactions GROUP BY interaction_channel;

## 5. Additional Analysis

*Identify the customers who made a purchase but did not churn.*

This task helps identify customers who continue to make purchases despite not churning. It can provide insights into factors contributing to customer loyalty.

    SELECT customers.customer_id, customers.name 
       FROM customers LEFT JOIN churn ON customers.customer_id = churn.customer_id 
       LEFT JOIN purchases ON customers.customer_id = purchases.customer_id 
       WHERE churn.customer_id IS NULL AND purchases.customer_id IS NOT NULL;
*Determine the most common interaction channel among customers who churned.*

Analyzing the most common interaction channel among churned customers helps understand their preferred mode of communication. 
It can guide efforts to improve customer engagement.

    SELECT interaction_channel, COUNT(*) AS channel_count 
      FROM interactions 
      INNER JOIN churn ON interactions.customer_id = churn.customer_id 
      GROUP BY interaction_channel 
      ORDER BY channel_count DESC 
      LIMIT 1;
*Calculate the average number of interactions before churning for customers who churned.* 

Analyzing the average number of interactions before churn helps identify the level of customer engagement leading up to churn. 
It provides insights into potential patterns or triggers for churn.

     SELECT AVG(interaction_count) AS average_interactions_before_churn 
       FROM (SELECT customer_id, COUNT(*) AS interaction_count 
       FROM interactions WHERE customer_id IN (SELECT DISTINCT customer_id FROM churn) 
       GROUP BY customer_id) AS subquery;     

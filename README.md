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

         

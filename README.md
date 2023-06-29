# Customer Churn Analysis: Uncovering Insights Using SQL

![](Churn-rate.jpg)

## INTRODUCTION

Customer churn analysis is an important activity for businesses of all sizes. It entails recognizing clients who are most likely to end their association with the business and comprehending the causes of churn. In order to gain useful insights, we will perform customer churn analysis using SQL in this project. We will examine a dataset of customer interactions and behavior.

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

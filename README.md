# Telecom Churn Analysis
 
# Table of Contents 
* [Problem Statement](#Problem-Statement)
* [Tasks](#Tasks)
* [Datasets, Characteristics, and Loading](#Datasets)
* [Recommendations](#Recommendations)
* [SQL Findings](#SQL-Findings)
* [Conclusion](#Conclusion)

## Problem Statement
A telecom company needs someone to analyze customers who have left their company. They want to determine: 
1. ***Who is churning?***
2. ***What factors are significantly contributing to churn?***
3. ***What can we do to reduce churn?***

## Tasks 
* Data exploration using SQL (Postgres) 
* Recommendations based on findings 

## Datasets
I downloaded these datasets from this [Kaggle page](https://www.kaggle.com/datasets/shilongzhuang/telecom-customer-churn-by-maven-analytics).
* Telecom Customer Churn
* Telecom Data Dictionary

## Dataset Characteristics
I have attached the data dictionary along with the first 4  of 7043 rows of data from the telecom customer churn dataset. The data contains telecom customer information, including demographics, usage patterns, service subscriptions, and financial details. 

<details>
    <summary>Telecom Data Dictionary</summary>
    <p>

| Field                          | Description                                                                                                                                                                        |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CustomerID                     | A unique ID that identifies each customer                                                                                                                                          |
| Gender                         | The customer's gender: Male, Female                                                                                                                                               |
| Age                            | The customer's current age, in years, at the time the fiscal quarter ended (Q2 2022)                                                                                            |
| Married                        | Indicates if the customer is married: Yes, No                                                                                                                                     |
| Number of Dependents           | Indicates the number of dependents that live with the customer (dependents could be children, parents, grandparents, etc.)                                                       |
| City                           | The city of the customer's primary residence in California                                                                                                                        |
| Zip Code                       | The zip code of the customer's primary residence                                                                                                                                  |
| Latitude                       | The latitude of the customer's primary residence                                                                                                                                  |
| Longitude                      | The longitude of the customer's primary residence                                                                                                                                 |
| Number of Referrals            | Indicates the number of times the customer has referred a friend or family member to this company to date                                                                         |
| Tenure in Months               | Indicates the total amount of months that the customer has been with the company by the end of the quarter specified above                                                        |
| Offer                          | Identifies the last marketing offer that the customer accepted: None, Offer A, Offer B, Offer C, Offer D, Offer E                                                                  |
| Phone Service                  | Indicates if the customer subscribes to home phone service with the company: Yes, No                                                                                             |
| Avg Monthly Long Distance Charges | Indicates the customer's average long-distance charges, calculated to the end of the quarter specified above (if the customer is not subscribed to home phone service, this will be 0) |
| Multiple Lines                 | Indicates if the customer subscribes to multiple telephone lines with the company: Yes, No (if the customer is not subscribed to home phone service, this will be No)              |
| Internet Service               | Indicates if the customer subscribes to Internet service with the company: Yes, No                                                                                                |
| Internet Type                  | Indicates the customer's type of internet connection: DSL, Fiber Optic, Cable (if the customer is not subscribed to internet service, this will be None)                          |
| Avg Monthly GB Download        | Indicates the customer's average download volume in gigabytes, calculated to the end of the quarter specified above (if the customer is not subscribed to internet service, this will be 0) |
| Online Security                 | Indicates if the customer subscribes to an additional online security service provided by the company: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Online Backup                   | Indicates if the customer subscribes to an additional online backup service provided by the company: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Device Protection Plan          | Indicates if the customer subscribes to an additional device protection plan for their Internet equipment provided by the company: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Premium Tech Support             | Indicates if the customer subscribes to an additional technical support plan from the company with reduced wait times: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Streaming TV                     | Indicates if the customer uses their Internet service to stream television programming from a third-party provider at no additional fee: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Streaming Movies                 | Indicates if the customer uses their Internet service to stream movies from a third-party provider at no additional fee: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Streaming Music                  | Indicates if the customer uses their Internet service to stream music from a third-party provider at no additional fee: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Unlimited Data                   | Indicates if the customer has paid an additional monthly fee to have unlimited data downloads/uploads: Yes, No (if the customer is not subscribed to internet service, this will be No) |
| Contract                         | Indicates the customer's current contract type: Month-to-Month, One Year, Two Year                                                                                                |
| Paperless Billing                 | Indicates if the customer has chosen paperless billing: Yes, No                                                                                                                 |
| Payment Method                    | Indicates how the customer pays their bill: Bank Withdrawal, Credit Card, Mailed Check                                                                                         |
| Monthly Charge                    | Indicates the customer's current total monthly charge for all their services from the company                                                                                   |
| Total Charges                     | Indicates the customer's total charges, calculated to the end of the quarter specified above                                                                                   |
| Total Refunds                     | Indicates the customer's total refunds, calculated to the end of the quarter specified above                                                                                   |
| Total Extra Data Charges           | Indicates the customer's total charges for extra data downloads above those specified in their plan, by the end of the quarter specified above                                   |
| Total Long Distance Charges        | Indicates the customer's total charges for long distance above those specified in their plan, by the end of the quarter specified above                                        |
| Total Revenue                      | Indicates the company's total revenue from this customer, calculated to the end of the quarter specified above (Total Charges - Total Refunds + Total Extra Data Charges + Total Long Distance Charges) |
| Customer Status                   | Indicates the status of the customer at the end of the quarter: Churned, Stayed, or Joined                                                                                      |
| Churn Category                     | A high-level category for the customer's reason for churning, which is asked when they leave the company: Attitude, Competitor, Dissatisfaction, Other, Price (directly related to Churn Reason) |
| Churn Reason                       | A customer's specific reason for leaving the company, which is asked when they leave the company (directly related to Churn Category)                                              |

</p>
</details>


<details>
    <summary>Telecom Customer Churn Dataset </summary>
    <p>

| Customer ID   | Gender | Age | Married | Number of Dependents | City         | Zip Code | Latitude   | Longitude     | Number of Referrals | Tenure in Months | Offer   | Phone Service | Avg Monthly Long Distance Charges | Multiple Lines | Internet Service | Internet Type | Avg Monthly GB Download | Online Security | Online Backup | Device Protection Plan | Premium Tech Support | Streaming TV | Streaming Movies | Streaming Music | Unlimited Data | Contract          | Paperless Billing | Payment Method    | Monthly Charge | Total Charges | Total Refunds | Total Extra Data Charges | Total Long Distance Charges | Total Revenue | Customer Status | Churn Category   | Churn Reason                 |
|---------------|--------|-----|---------|-----------------------|--------------|----------|------------|---------------|---------------------|------------------|---------|----------------|----------------------------------|-----------------|-------------------|---------------|--------------------------|------------------|---------------|--------------------------|-----------------------|---------------|------------------|------------------|-----------------|-------------------|---------------------|----------------------|----------------|---------------|---------------|--------------------------|----------------------------|---------------|------------------|------------------|-----------------------------|
| 0002-ORFBO    | Female | 37  | Yes     | 0                     | Frazier Park | 93225    | 34.827662  | -118.999073  | 2                   | 9                | None    | Yes            | 42.39                            | No              | Yes               | Cable         | 16                       | No               | Yes           | No                       | Yes                   | Yes           | No               | No               | Yes             | One Year           | Yes                | Credit Card        | 65.6           | 593.3         | 0             | 0                        | 381.51                     | 974.81        | Stayed           |                  |                              |
| 0003-MKNFE    | Male   | 46  | No      | 0                     | Glendale     | 91206    | 34.162515  | -118.203869  | 0                   | 9                | None    | Yes            | 10.69                            | Yes             | Yes               | Cable         | 10                       | No               | No            | No                       | No                    | No            | No               | Yes              | No              | Month-to-Month     | No                 | Credit Card        | -4             | 542.4         | 38.33         | 10                       | 96.21                      | 610.28        | Stayed           |                  |                              |
| 0004-TLHLJ    | Male   | 50  | No      | 0                     | Costa Mesa   | 92627    | 33.645672  | -117.922613  | 0                   | 4                | Offer E | Yes            | 33.65                            | No              | Yes               | Fiber Optic   | 30                       | No               | No            | Yes                      | No                    | No            | No               | No               | Yes             | Month-to-Month     | Yes                | Bank Withdrawal   | 73.9           | 280.85        | 0             | 0                        | 134.6                      | 415.45        | Churned          | Competitor       | Competitor had better devices |
| 0011-IGKFF    | Male   | 78  | Yes     | 0                     | Martinez     | 94553    | 38.014457  | -122.115432  | 1                   | 13               | Offer D | Yes            | 27.82                            | No              | Yes               | Fiber Optic   | 4                        | No               | Yes           | Yes                      | No                    | Yes           | Yes              | Yes              | No              | Month-to-Month     | Yes                | Bank Withdrawal   | 98             | 1237.85       | 0             | 0                        | 361.66                     | 1599.51       | Churned          | Dissatisfaction  | Product dissatisfaction     |

</p>
</details>


## Dataset Loading
After ensuring the dataset was clean, I loaded the telecom customer churn dataset into pgAdmin4 to begin the analysis.

<details>
    <summary>DDL and Import Code</summary>
    <p>

```sql
CREATE TABLE telecom_customer_churn(
Customer_id VARCHAR(20),
Gender VARCHAR(10),
Age INTEGER,
Married VARCHAR(3),
Number_of_Dependents INTEGER,
City VARCHAR(50),
Zipcode VARCHAR(10),
Latitude DOUBLE PRECISION,
Longitude DOUBLE PRECISION,
Number_of_referrals INTEGER,
Tenure_in_months INTEGER,
Offer VARCHAR(10),
Phone_Service VARCHAR(3),
Avg_Monthly_Long_Distance_Charges DOUBLE PRECISION,
Multiple_Lines VARCHAR(3),
Internet_Service VARCHAR(20),
Internet_Type VARCHAR(20),
Avg_Monthly_GB_Download INTEGER,
Online_Security VARCHAR(3),
Online_Backup VARCHAR(3),
Device_Protection_Plan VARCHAR(3),
Premium_Tech_Support VARCHAR(3),
Streaming_TV VARCHAR(3),
Streaming_Movies VARCHAR(3),
Streaming_Music VARCHAR(3),
Unlimited_Data VARCHAR(3),
Contract VARCHAR(20),
Paperless_Billing VARCHAR(3),
Payment_Method VARCHAR(20),
Monthly_Charge DOUBLE PRECISION,
Total_Charges DOUBLE PRECISION,
Total_Refunds DOUBLE PRECISION,
Total_Extra_Data_Charges DOUBLE PRECISION,
Total_Long_Distance_Charges DOUBLE PRECISION,
Total_Revenue DOUBLE PRECISION,
Customer_Status VARCHAR(20),
Churn_Category VARCHAR(20),
Churn_Reason VARCHAR(100)
);

COPY telecom_customer_churn
FROM '/Users/dustintucker/Downloads/telecom_customer_churn.csv' 
WITH CSV HEADER DELIMITER ',';
```
</p>
</details>

# Recommendations
## What can we do to reduce churn? 

### 1. Optimize Contract Types, Tenure, and Referrals:
* Encourage longer contract commitments by offering incentives for one-year and two-year contracts. This will in turn increase tenure, a direct driver in creating lower churn.
* Implement targeted promotions for month-to-month customers to increase retention, potentially with loyalty-based rewards.
* Implement referral programs to increase referrals, as higher referrals correlate with higher retention. 

### 2. Focus on Competitive Factors:
* Address the top churn reason, customers leaving for better devices or offers from competitors, by conducting regular competitor analyses to ensure the company remains competitive in terms of services, products, pricing, and offers.

### 3. Enhance Customer Engagement through Premium Services:
* Promote premium internet services to improve customer satisfaction and reduce churn.
* Place emphasis on online security and premium tech support due to higher churn when a customer does not have these premium services.
* Consider bundling premium services into attractive packages to encourage customers to upgrade.

### 4. Tailor Offers to Specific Customer Segments: 
* Focus on understanding the unique needs and preferences of non-married individuals, people with no dependents, different age groups (specifically ages 65+), and residents in high-churn cities such as San Diego. 

### 5. Understand Current Offers:
* Further look into why Offer F is performing poorly and decide whether to continue promoting Offer F or not. 
* Consider prioritizing Offer A and B due to their low churn and high revenue. 

### 6. Improve Customer Support:
* Address the concerns related to customer support attitudes, as identified in churn reasons.
* Consider implementing training programs to enhance customer service skills that emphasize the importance of positive interactions.

# SQL Findings 
* I broke the problem statement into 9 sections with questions that would guide my exploration with the focus of answering the problem statement and reasons for my recommendations. 
* Here, I will go over the *key* findings in SQL that both answer the problem statement along with findings that led to my recommendations. 
* To view all sections, questions, and code [click here](https://github.com/dustin-tucker01/churnanalysis/tree/main/SQL%20code%20%26%20Questions%20). 

## Who is churning? 

**Questions:**
*   How are demographic features distributed among the churned customers?
*   How does the churn rate vary by different demographic factors (e.g. [gender](#1-gender), [age](#2-age-groups), [marital status](#3-marriage), [dependents](#4-dependents), [city](#5-cities))?  

### 1. **Gender**
````sql
SELECT Gender,
CONCAT(ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2), '%') AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY Gender;
````
Output:

<img width="543" alt="Screenshot 2023-12-28 at 10 00 34 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/82c703fd-3ef3-4d5d-a814-b664229e4e1e">


*Insights: Churners are distributed pretty evenly based on gender. Additionally, churn rate is not affected by gender.*

###  2. **Age Groups**
````sql
WITH age_groups AS 
(SELECT customer_id, customer_status, 
 CASE WHEN age BETWEEN 18 AND 24 THEN '18-24'
      WHEN age BETWEEN 25 AND 34 THEN '25-34'
      WHEN age BETWEEN 35 AND 44 THEN '35-44'
      WHEN age BETWEEN 45 AND 54 THEN '45-54'
      WHEN age BETWEEN 55 AND 64 THEN '55-64'
      WHEN age >= 65 THEN '65+' END AS age_range,
CASE  WHEN age BETWEEN 18 AND 24 THEN 1
      WHEN age BETWEEN 25 AND 34 THEN 2
      WHEN age BETWEEN 35 AND 44 THEN 3
      WHEN age BETWEEN 45 AND 54 THEN 4
      WHEN age BETWEEN 55 AND 64 THEN 5
      WHEN age >= 65 THEN 6 END AS age_sort 
FROM telecom_customer_churn)
SELECT age_range, 
CONCAT(ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2), '%') AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM age_groups
GROUP BY age_range, age_sort
ORDER BY age_sort;
````
Output:

<img width="484" alt="Screenshot 2024-01-04 at 3 02 52 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/56468ee3-9036-4416-a31b-883a062ffc48">


*Insights: Most of the churned customers are between the 25-65+ age range with a small portion in the 18-24 age range. Customers in the age range of 65+ are more likely to leave with a churn rate of over 40% compared to other age groups in the 20-25% churn rate range.*

### 3. **Marriage**
````sql
SELECT married,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY married
ORDER BY churn_rate DESC;
````
Output: 

<img width="541" alt="Screenshot 2023-12-28 at 10 01 31 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/c9f697d9-581d-4abc-a0f3-f20cad4f747c">


*Insights: Most of the churners are not married. Customers who are not married are more likely to churn with a churn rate of 32%.* 

### 4. **Dependents**
````sql
SELECT number_of_dependents,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY number_of_dependents 
ORDER BY number_of_dependents;
````
Output:

<img width="553" alt="Screenshot 2024-01-05 at 1 34 23 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/10005103-68df-46b0-bbbc-1da4af5727be">


*Insights: Most churners have 0-3 dependents, with the majority of churners having 0 dependents*

### 5. **Cities**
````sql
SELECT city,
CONCAT(ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2), '%') AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY city
ORDER BY churned_customers DESC
LIMIT 5;
````
Output:

<img width="549" alt="Screenshot 2024-01-04 at 3 03 58 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/466dcecd-1f47-4d4a-bf57-9d998589bffd">
 

*Insights: Most of the churned customers are from these top 5 cities (listed above). There are too many total citys (1106 total) to draw conclusions of churn rates based on city, but San Diego has a high number of churned customers and a churn rate of over 64%. I wanted to further break down San Diego into zipcodes to determine which zipcodes were resulting in the most churns.*

### 6. **San Diego Zipcodes**
````sql
SELECT city, zipcode,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
WHERE city = 'San Diego'
GROUP BY city, zipcode 
ORDER BY churned_customers DESC
LIMIT 5;
````
Output:

<img width="699" alt="Screenshot 2023-12-28 at 10 52 20 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/756a44e1-9c4b-470f-9ff8-ddb1f600c0c0">


*Insights: I identified that these five zipcodes (listed above) in San Diego were resulting with the most churned customers. Interestingly, these zipcodes all had churn rates of over 80% along with the most churned customers. San Diego is clearly a pain point in the customer base.*

## What factors are significantly contributing to churn?
**Questions:**
* [Tenures](#1-tenures): Is there a correlation between tenure and churn rate?
* [Referrals](#2-referrals): Is there any correlation between the number of referrals a customer has made and their likelihood of churning?
* [Internet Service](#3-internet-service): Does the type of service a customer buys affect churn rates?
* [Premium Internet Services](#4-premium-internet-services): Do customers with certain types of premium internet services have a higher or lower churn rate?
* [Offers](#5-offers): How does the churn rate differ based on the type of offer the customer is on?
* [Contract Types](#6-contract-types): What is the churn rate for customers with different contract types (e.g., month-to-month vs. long-term contracts)?
* [Churn Category](#7-churn-category): What are the top categories for why customers churned?
* [Churn Reason](#8-churn-reason): What were the top reasons for leaving the company among those who churned?

### 1. Tenures
````sql
WITH distinct_tenures AS 
(SELECT DISTINCT(tenure_in_months) AS tenure_in_months
FROM telecom_customer_churn),
tenure_buckets AS 
(SELECT tenure_in_months, NTILE(6) OVER (ORDER BY tenure_in_months) AS tenure_buckets
FROM distinct_tenures),
tenure_groups AS 
(SELECT tenure_in_months, tenure_buckets, CONCAT(MIN(tenure_in_months) OVER(PARTITION BY tenure_buckets), '-', MAX(tenure_in_months) OVER(PARTITION BY tenure_buckets), ' Months') AS tenure_ranges
FROM tenure_buckets),
joined_tables AS
(SELECT t1.customer_id, t1.customer_status, t2.tenure_ranges, t2.tenure_buckets
FROM telecom_customer_churn t1
JOIN tenure_groups t2
ON t1.tenure_in_months = t2.tenure_in_months)
SELECT tenure_ranges, CONCAT(ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2), '%') AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM joined_tables
GROUP BY tenure_ranges, tenure_buckets
ORDER BY tenure_buckets;
````
Output: 

<img width="502" alt="Screenshot 2023-12-28 at 10 04 00 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/0a8c190e-cc64-4785-ba6a-94ef1e49032e">

*Insights: There is a clear correlation between tenure and churn rates. Churn is highest in the 1-12 Month tenure range. Churn rates drop significantly lower with each increase in tenure range.*

### 2. Referrals
````sql
WITH distinct_referrals AS 
(SELECT DISTINCT(number_of_referrals) AS number_of_referrals
FROM telecom_customer_churn),
referral_buckets AS 
(SELECT number_of_referrals , NTILE(4) OVER (ORDER BY number_of_referrals) AS referral_buckets
FROM distinct_referrals),
referral_groups AS 
(SELECT number_of_referrals, referral_buckets, 
CONCAT(MIN(number_of_referrals) OVER(PARTITION BY referral_buckets), '-', MAX(number_of_referrals) OVER(PARTITION BY referral_buckets)) AS referral_ranges
FROM referral_buckets),
joined_tables AS
(SELECT t1.customer_id, t1.customer_status, t2.referral_ranges, t2.referral_buckets
FROM telecom_customer_churn t1
JOIN referral_groups t2
ON t1.number_of_referrals = t2.number_of_referrals)
SELECT referral_ranges, CONCAT(ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2), '%') AS ChurnRate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1  ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM joined_tables 
GROUP BY referral_ranges, referral_buckets
ORDER BY referral_buckets;
````
Output: 

<img width="500" alt="Screenshot 2024-01-04 at 3 05 39 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/a99511ac-6efc-4144-95ae-d0ccacd4dffb">


*Insights: Similarly to tenures, higher referrals correlate with lower churn.* 

### 3. Internet Service
````sql
SELECT internet_service,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY internet_service
ORDER BY churn_rate DESC;
````
Output:

<img width="549" alt="Screenshot 2023-12-28 at 10 20 14 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/c33593b3-e901-4b37-b0b7-f3e194c8fa33">


*Insights: The churn rate for internet service is higher, but most of the customer base uses the internet service so I wanted to dig deeper into if premium internet services such as online security, online backup, device protection plan, premium tech support, streaming tv, streaming movies, streaming music, or unlimited data had any effect on churn rates.*

### 4. Premium Internet Services
* **Online Security**
````sql
SELECT Online_Security,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
WHERE Online_Security IS NOT NULL
GROUP BY Online_Security
ORDER BY churn_rate DESC;
````
Output:

<img width="543" alt="Screenshot 2023-12-28 at 10 22 23 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/4deb5c91-ec01-487d-a89f-0d2226c330df">


* **Premium Tech Support** 
````sql
SELECT Premium_Tech_Support,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
WHERE Premium_Tech_Support IS NOT NULL
GROUP BY Premium_Tech_Support
ORDER BY churn_rate DESC;
````
Output: 

<img width="539" alt="Screenshot 2023-12-28 at 10 22 57 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/09469f1f-1079-45ac-a4d5-a18f80cbe841">


*Insights: I thoroughly looked into each premium internet service, and I found that each premium feature offers lower churn rates when a customer has it compared to when they do not. Customers who do not have online security and premium tech support (listed above) are significantly more likely to churn at rates of over 40% in each.*

### 5. Offers
````sql
SELECT offer, 
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4))/COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers,
CAST(CAST(SUM(total_revenue) AS DECIMAL(20,2)) AS MONEY) AS revenue_per_offer, 
CAST(CAST(SUM(total_revenue) AS DECIMAL(20,2))/COUNT(Customer_ID) AS MONEY) AS revenue_per_customer
FROM telecom_customer_churn
GROUP BY offer
ORDER BY churn_rate DESC;
````
Output: 

<img width="818" alt="Screenshot 2023-12-28 at 10 19 27 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/d617c2d7-b6a8-4396-9f13-3d9f99b61549">


*Insights: Offer E proves to result in a high churn rate of 52%. This offer has the least amount of revenue and the least amount of revenue per customer. Offer A and B seem promising due to low churn and high revenue.*  

### 6. Contract Types
````sql
SELECT contract,
ROUND(CAST(SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS DECIMAL(10,4)) / COUNT(Customer_ID) * 100, 2) AS churn_rate,
SUM(CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END) AS churned_customers,
COUNT(Customer_ID) AS total_customers
FROM telecom_customer_churn
GROUP BY contract
ORDER BY churn_rate DESC;
````
Output: 

<img width="551" alt="Screenshot 2023-12-28 at 10 19 54 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/85bc218b-57d5-40c9-bf40-2792d90a4220">


*Insights: The longer the contract, the lower the churn. A month-to-month contract results in a 45% churn rate. One year and two year contracts should be the focus in lowering churn for this feature.* 

### 7. Churn Category
````sql
SELECT churn_category, 
COUNT(churn_category) AS total_customers, 
CONCAT(ROUND((CAST(COUNT(churn_category) AS DECIMAL(10,4))/(SELECT SUM(CASE WHEN churn_category IS NOT NULL THEN 1  ELSE 0 END) FROM telecom_customer_churn)) * 100,2), '%') AS percent_of_churned_customers 
FROM telecom_customer_churn
WHERE churn_category IS NOT NULL
GROUP BY churn_category
ORDER BY total_customers DESC;
````

Output: 

<img width="537" alt="Screenshot 2023-12-28 at 10 17 05 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/2fb1e1c9-3d07-4aaa-9452-837216f7eb7b">



*Insights: Almost half of churned customers left because of a competitor. I wanted to look into the specific reasons customers left.*

### 8. Churn Reason
````sql
SELECT churn_category, churn_reason, COUNT(churn_reason) AS total_customers, 
CONCAT(ROUND((CAST(COUNT(churn_reason) AS DECIMAL(10,4))/(SELECT SUM(CASE WHEN churn_category IS NOT NULL THEN 1 END) FROM telecom_customer_churn)) * 100,2), '%') AS percent_of_churned_customers
FROM telecom_customer_churn
WHERE churn_category IS NOT NULL
GROUP BY churn_category, churn_reason
ORDER BY total_customers DESC
LIMIT 5;
````
Output: 

<img width="723" alt="Screenshot 2023-12-28 at 10 18 45 PM" src="https://github.com/dustin-tucker01/ChurnAnalysis/assets/148160789/aaed68d6-e267-48ee-97f7-7523f62c079c">


*Insights: Over 45% of churned customers either left because:*
1. *Competitiors had better devices or made a better offer*
2. *The attitude of the support person they were in contact with was poor.*

## Conclusion 
<details>
 <summary>Key Insights Recap</summary>
<p>
	
### 1. Gender
* Churners are distributed evenly based on gender.
* Churn rate is not significantly affected by gender.

### 2. Age Groups
* Most churned customers are between the ages of 25-65+, with a small portion in the 18-24 age range.
* Customers aged 65+ have a higher likelihood of churning (over 40%).

### 3. Marriage
* Non-married individuals are more likely to churn, with a churn rate of 32%.

### 4. Dependents
* Most churners have 0-3 dependents, with the majority of churners having 0 dependents.

### 5. Cities
* Most churned customers are from San Diego, Los Angeles, San Francisco, San Jose, or Sacramento. San Diego is the only city with a high amount of churned customers and a high churn rate of 64%.  

### 6. San Diego Zipcodes
* These 5 zip codes in San Diego have the most churned customers, all with churn rates over 80%.
1. 92122
2. 92117
3. 92126
4. 92109
5. 92130

### 7. Tenures
* There is a clear correlation between tenure and churn rates.
* Churn is highest in the 1-12 month tenure range.

### 8. Referrals
* Higher referral numbers are associated with lower churn rates.

### 9. Internet Service
* The churn rate for internet service is higher, but further investigation into premium services was needed.

### 10. Premium Internet Services
* All premium features are associated with lower churn. Online security and premium tech support have the highest churn rates when a customer does not have them. 

### 11. Offers
* Offer E results in a high churn rate of 52%, while Offer A and B  have low churn and high revenue.

### 12. Contract Types
* Longer contracts have lower churn rates. Month-to-month contracts result in a 45% churn rate.

### 13. Churn Category
* Almost half of churned customers left due to competition.

### 14. Churn Reason
* Over 45% of churned customers left due to competitors offering better devices, competitors having better offers, or poor support person attitude.
  
</p>
</details>

I then used these key insights I derived from these queries to provide my recommendations in answering [what can we do to reduce churn?](#Recommendations) 

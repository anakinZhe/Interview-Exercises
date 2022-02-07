# FRCodeExercise

## First: New Structured Relational Data Model

<img width="1408" alt="Screen Shot 2022-02-07 at 1 59 21 PM" src="https://user-images.githubusercontent.com/47021692/152853852-e1d5f5fd-7159-4815-8738-5ba77affcd27.png">

## Second: Write a query that directly answers a predetermined question from a business stakeholder
The query below answers those two questions:

- When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
- When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?

```mysql
/*mysql*/
select
  rewards_receipt_status,
  sum(purchased_item_count) as total_number_of_items_purchased,
  avg(total_spent) as average_spend
from receipt
where rewards_receipt_status in ('Accepted', 'Rejected')
group by rewards_receipt_status
```

## Third: Evaluate Data Quality Issues in the Data Provided
After reviewing the data provided, I found those data quality issues that might cause trouble to business analysis:
- For the **users** file, there are a huge amount of duplicates and some outliers that need to be evaluated and cleaned. The duplicates might come from merging data from different sources, and the outliers might come from testing activities. Those outliers should be deleted before future analysis or query.
- Data accuracy is a big problem for records in **brands** file. It is caused by missing values in the key field ```brandCode```, a mismatch between ```category``` and ```categoryCode```, and some duplicates in ```name```, ```barcode```, and ```brandCode```.
- The main part of the **receipt** file looks like a combination of two tables, one of which is used only to store information like ```rewardsReceiptStatus``` and have missing values for all other fields. For the **itemList** part, the two major problems are data validility and missing values for key fields. I think every record should have at least certain variables so the record are searchable and joinable. For now, only the ```partnerItemId``` field doesn't have missing values.

Please refer to [FetchRewards.ipynb](FetchRewards.ipynb) for the analysis with codes.

## Fourth: Communicate with Stakeholders

Dear Product Manager,

I am a data analyst in the analyst team. While working on the receipts data that are generated by users and brands data in our database, I found some data quality issues that can be solved to improve accuracy. Also, I have some questions on those data that you might be able to help with. 

First, I found the brand data in our system have a lot of missing values and mismatch, making it hard to track user activities for different brands. I want to know where are those brand data come from, how they are generated, and whether they are checked and adjusted before putting into use. Those information allows me to find the way to improve those data. 

Apart from that, I found that the item on receipts lacks enough information for us to make use of them. For example, the receipts we get from supermarkets and grocery stroes should have the name, price, and maybe barcode for each item. These should be the same for records in our database. However, I found a lot of missing values while working on it. It might because our app are not able to recognize those information while scanning, or we didn't store relevant inforamtion at that time. This will make those receipts we collect from users useless. For the engineer team or the product design team, I suggest asking users to fill the missing values and asking the system to keep necessary information. In the meanwhile, I want to know what database does the system use to identity items and brands for each record. This can help me optimize the current dataset even if we don't imporve the scanning process. 

Apart from that, I found that the item on receipts lacks enough information for us to make use of them. For example, the receipts we get from supermarkets and grocery stroes should have the name, price, and maybe barcode for each item. These should be the same for records in our database. However, I found a lot of missing values while working on it. It might because our app are not able to recognize those information while scanning, or we didn't store relevant inforamtion at that time. This will make those receipts we collect from users useless. For the engineer team or the product design team, I suggest asking users to fill the missing values and asking the system to keep necessary information. In the meanwhile, I want to know what database does the system use to identity items and brands for each record. This can help me optimize the current dataset even if we don't imporve the scanning process.

Please let me know your thoughts and if you need more information from me. 

Best,
Zhengzhe Jia

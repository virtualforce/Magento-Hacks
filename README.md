# Magento-Hacks
I will explain some of minor tricks that i used while developing an online store in magento 1.9. 

## 1. Adding City to Shopping Cart Price Rules:
How you can add shipping city to shopping cart rules in magento 1.9?

#### Scenario:
For example You want to set free shipping for specific city or you want to apply specific discount on the total price of cart against certain city.

You will simple add a new rule from Dashboard->Promotions->Shopping Cart Price Rules-> Add new Rule.

![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin_shopping_menu.png "Adding New Rule for Shopping cart")

Here you will fill up the general information about this rule. Now if you switch to conditions tab as show below.Here you see a problem. You want to add condition for shipping city which is not available in the dropdown by default.

![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin_shoppiing_no_city.png "City is not listed dow by default")

#### Solution:
You need to copy `Address.php` from `app/code/core/mage/SalesRule/Model/Rule/Condition/` to your local repository of extentions and that will be `app/code/local/Mage/SalesRule/Model/Rule/Condition/`. Final path to file should be like `app/code/local/Mage/SalesRule/Model/Rule/Condition/Address.php`.

Now open this file and add below line.
```php
'city' => Mage::helper('salesrule')->__('Shipping City'),
```

in the attributes array as shown below.

![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin-shopping_city_file.png "Add above line to attributes array").

Save your file and now refresh the page again and add condition. Now you will see the shipping city  option in the dropdown as shown below.

![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin-shopping_city.png "City listed").

## 2. Emails in Queue Issue:

Some times you are not recieving emails instantly either on admin or customer side.

#### Scenario:
Any customer place an order on your magento store and you are not recieving emails ?

#### Solution:
Infact Magento put the emails in queue and then send the emails one by one. Most of the time, it seems to be cronjob issue. So you clear the cache and run the cronjob manually. But still, if you are not recieving emails, then only solution is to stop queuing emails. And how you do that is below.

You need to copy `Order.php` from `app/code/core/mage/Sales/Model/` to your local repository of extentions and that will be `app/code/local/Mage/Sales/Model/`. Final path to file should be like `app/code/local/Mage/Sales/Model/Order.php`.

Now open this file and go to line 1354 and 1450. Here you the see the code that put emails in queue ads below.

```php
$mailer->setQueue($emailQueue)->send();
```
Remove/Replace this line with below one.

```php
$mailer->send();
```

Save your file and now refresh the page again and thats it. You will be recieving emails at the moment order is placed.

## 3. Optimize magento database via phpMyAdmin or mysql client:

Magento maintains several tables for logging.Magento has a mechanism for cleaning these logs regularly. I am going to tell you how we can clean out specific log tables via phpMyAdmin or mysql client manually.There are other ways too.

#### Scenario:
The following tables are managed by Magento’s Log Cleaning function:

```php
log_customer
log_visitor
log_visitor_info
log_url
log_url_info
log_quote
report_viewed_product_index
report_compared_product_index
report_event
catalog_compare_item
```

#### Solution:
Let ‘s execute this SQL to clear and optimize them

```php
SET foreign_key_checks = 0;
TRUNCATE dataflow_batch_export;
TRUNCATE dataflow_batch_import;
TRUNCATE log_customer;
TRUNCATE log_quote;
TRUNCATE log_summary;
TRUNCATE log_summary_type;
TRUNCATE log_url;
TRUNCATE log_url_info;
TRUNCATE log_visitor;
TRUNCATE log_visitor_info;
TRUNCATE log_visitor_online;
TRUNCATE report_viewed_product_index;
TRUNCATE report_compared_product_index;
TRUNCATE report_event;
TRUNCATE index_event;
TRUNCATE catalog_compare_item;
SET foreign_key_checks = 1;
```

## 4. Magento Admin Login issue:
when I tried to login to the Magento admin panel using Google Chrome and I typed username and password, clicked Login button and nothing happened. The page refreshes and that’s all. No error or any other messages.

#### Solution:
Run this Sql

```php
SET FOREIGN_KEY_CHECKS=0;
UPDATE core_store SET store_id = 0 WHERE code='admin';
UPDATE core_store_group SET group_id = 0 WHERE name='Default';
UPDATE core_website SET website_id = 0 WHERE code='admin'; 
UPDATE customer_group SET customer_group_id = 0 WHERE customer_group_code='NOT LOGGED IN';
SET FOREIGN_KEY_CHECKS=1;
```

And that's all.
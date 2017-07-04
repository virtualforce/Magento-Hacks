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
You need to copy Address.php from app/code/core/mage/SalesRule/Model/Rule/Condition/ to your local repository of extentions and that will be app/code/local/SalesRule/Model/Rule/Condition/. Final path to file should be like app/code/core/mage/SalesRule/Model/Rule/Condition/Address.php.

Now open this file and add below line.
'city' => Mage::helper('salesrule')->__('Shipping City'),

in the attributes array as shown below.
![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin-shopping_city_file.png "Add above line to attributes array").

Save your file and now refresh the page again and add condition. Now you will see the shipping city  option in the dropdown as shown below.
![alt text](https://github.com/virtualforce/Magento-Hacks/blob/master/images/mage_admin-shopping_city.png "City listed").
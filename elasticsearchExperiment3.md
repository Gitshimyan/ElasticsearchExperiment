# 实验三 聚合操作练习

### 学院：省级示范性软件学院

### 题目：《 实验三：索引&文档操作》

### 姓名：晏仕民

### 学号：2200770068

### 班级：软工2201

### 日期：2024-10-15

### 实验环境： elasticsearch-8.12.2   kibana-8.12.2

## 一、实验目的：

### 掌握Elasticsearch 聚合操作方法

## 二、实验内容：

### Elasticsearch 聚合操作练习

***
***--完成题目中的任意15道题--***

***1. 统计每个产品类别的总销售额。*** 

***2. 计算每个城市的平均订单金额。*** 

***3. 找出销量最高的前5个产品。*** 

***4. 计算男性和女性客户的平均年龄。*** 

***5. 统计每种支付方式的使用次数和总金额。*** 

***6. 计算每月的总销售额。*** 

***7. 找出平均订单金额最高的前3个客户。*** 

***8. 计算每个年龄段（18-30，31-50，51+）的客户数量。*** 

***9. 计算每个产品类别的平均单价。*** 

***10. 找出订单数量最多的前5个城市。*** 

***11. 计算每个季度的平均订单金额。*** 

***12. 统计每个产品类别中的商品数量。*** 

***13. 计算男性和女性客户的平均订单金额。*** 

***14. 找出总销售额最高的前10个日期。*** 

***15. 计算每个季度销售额最高的产品类别。*** 

***16. 计算每天的订单数量，并显示7天移动平均值。*** 

***17. 比较本月销售额与上月销售额的差异。*** 

***18. 计算每周的总销售额，并找出销售额增长最快的一周。***
***

### 创建索引--
```
PUT /ecommerce
{
  "mappings": {
    "properties": {
      "order_id": { "type": "keyword" },
      "order_date": { "type": "date" },
      "customer_id": { "type": "keyword" },
      "customer_name": { "type": "keyword" },
      "customer_gender": { "type": "keyword" },
      "customer_age": { "type": "integer" },
      "customer_city": { "type": "keyword" },
      "product_id": { "type": "keyword" },
      "product_name": { "type": "keyword" },
      "product_category": { "type": "keyword" },
      "quantity": { "type": "integer" },
      "price": { "type": "float" },
      "total_amount": { "type": "float" },
      "payment_method": { "type": "keyword" },
      "is_returned": { "type": "boolean" }
    }
  }
}
```

### 结果--
```
{
  "error": {
    "root_cause": [
      {
        "type": "resource_already_exists_exception",
        "reason": "index [ecommerce/xDPH7CB5RRW_Wd4mFE7jPQ] already exists",
        "index_uuid": "xDPH7CB5RRW_Wd4mFE7jPQ",
        "index": "ecommerce"
      }
    ],
    "type": "resource_already_exists_exception",
    "reason": "index [ecommerce/xDPH7CB5RRW_Wd4mFE7jPQ] already exists",
    "index_uuid": "xDPH7CB5RRW_Wd4mFE7jPQ",
    "index": "ecommerce"
  },
  "status": 400
}
```
### 示例图片--
![Markdown Logo](./Images/1.png)
### 数据引入--
```
POST /ecommerce/_bulk
{"index": {"_id": "1"}}
{"order_id": "6945", "order_date": "2023-03-06", "customer_id": "C242", "customer_name": "Bob Smith", "customer_gender": "female", "customer_age": 60, "customer_city": "Philadelphia", "product_id": "P041", "product_name": "Pro Accessory", "product_category": "Sports", "quantity": 4, "price": 205.89, "total_amount": 823.56, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "2"}}
{"order_id": "5629", "order_date": "2023-11-14", "customer_id": "C262", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 52, "customer_city": "Dallas", "product_id": "P097", "product_name": "Ultra Accessory", "product_category": "Sports", "quantity": 3, "price": 475.18, "total_amount": 1425.54, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "3"}}
{"order_id": "4488", "order_date": "2023-03-28", "customer_id": "C342", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 62, "customer_city": "New York", "product_id": "P001", "product_name": "Ultra Device", "product_category": "Books", "quantity": 5, "price": 722.89, "total_amount": 3614.45, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "4"}}
{"order_id": "7960", "order_date": "2023-09-23", "customer_id": "C408", "customer_name": "Eva Johnson", "customer_gender": "male", "customer_age": 37, "customer_city": "San Antonio", "product_id": "P079", "product_name": "Ultra Accessory", "product_category": "Home Appliances", "quantity": 2, "price": 485.3, "total_amount": 970.6, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "5"}}
{"order_id": "2111", "order_date": "2023-02-18", "customer_id": "C981", "customer_name": "Eva Garcia", "customer_gender": "female", "customer_age": 19, "customer_city": "Los Angeles", "product_id": "P091", "product_name": "Mega Widget", "product_category": "Books", "quantity": 5, "price": 61.52, "total_amount": 307.6, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "6"}}
{"order_id": "2121", "order_date": "2023-01-10", "customer_id": "C030", "customer_name": "Jack Davis", "customer_gender": "male", "customer_age": 18, "customer_city": "Chicago", "product_id": "P070", "product_name": "Ultra Tool", "product_category": "Home Appliances", "quantity": 3, "price": 860.58, "total_amount": 2581.74, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "7"}}
{"order_id": "1211", "order_date": "2023-07-06", "customer_id": "C672", "customer_name": "Jack Williams", "customer_gender": "female", "customer_age": 64, "customer_city": "San Diego", "product_id": "P092", "product_name": "Mega Device", "product_category": "Electronics", "quantity": 4, "price": 389.1, "total_amount": 1556.4, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "8"}}
{"order_id": "7394", "order_date": "2023-08-15", "customer_id": "C719", "customer_name": "Frank Rodriguez", "customer_gender": "female", "customer_age": 59, "customer_city": "San Diego", "product_id": "P083", "product_name": "Super Device", "product_category": "Beauty", "quantity": 2, "price": 593.15, "total_amount": 1186.3, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "9"}}
{"order_id": "7593", "order_date": "2023-07-22", "customer_id": "C145", "customer_name": "Bob Johnson", "customer_gender": "female", "customer_age": 33, "customer_city": "Dallas", "product_id": "P031", "product_name": "Pro Tool", "product_category": "Furniture", "quantity": 3, "price": 740.32, "total_amount": 2220.96, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "10"}}
{"order_id": "3304", "order_date": "2023-03-03", "customer_id": "C111", "customer_name": "Jack Jones", "customer_gender": "female", "customer_age": 53, "customer_city": "Chicago", "product_id": "P027", "product_name": "Elite Device", "product_category": "Jewelry", "quantity": 1, "price": 489.24, "total_amount": 489.24, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "11"}}
{"order_id": "7928", "order_date": "2023-05-02", "customer_id": "C441", "customer_name": "Henry Williams", "customer_gender": "male", "customer_age": 57, "customer_city": "San Diego", "product_id": "P005", "product_name": "Elite Accessory", "product_category": "Groceries", "quantity": 3, "price": 989.17, "total_amount": 2967.51, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "12"}}
{"order_id": "9071", "order_date": "2023-04-01", "customer_id": "C798", "customer_name": "Grace Williams", "customer_gender": "male", "customer_age": 66, "customer_city": "Phoenix", "product_id": "P055", "product_name": "Ultra Widget", "product_category": "Furniture", "quantity": 4, "price": 332.32, "total_amount": 1329.28, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "13"}}
{"order_id": "4022", "order_date": "2023-08-04", "customer_id": "C763", "customer_name": "Jack Martinez", "customer_gender": "female", "customer_age": 23, "customer_city": "Phoenix", "product_id": "P026", "product_name": "Ultra Widget", "product_category": "Books", "quantity": 5, "price": 826.39, "total_amount": 4131.95, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "14"}}
{"order_id": "1363", "order_date": "2023-04-28", "customer_id": "C727", "customer_name": "Frank Davis", "customer_gender": "female", "customer_age": 67, "customer_city": "New York", "product_id": "P012", "product_name": "Super Widget", "product_category": "Toys", "quantity": 3, "price": 248.55, "total_amount": 745.65, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "15"}}
{"order_id": "6142", "order_date": "2023-04-27", "customer_id": "C005", "customer_name": "Charlie Davis", "customer_gender": "male", "customer_age": 28, "customer_city": "San Jose", "product_id": "P048", "product_name": "Ultra Gadget", "product_category": "Home Appliances", "quantity": 4, "price": 510.43, "total_amount": 2041.72, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "16"}}
{"order_id": "2769", "order_date": "2023-01-03", "customer_id": "C393", "customer_name": "Alice Rodriguez", "customer_gender": "male", "customer_age": 19, "customer_city": "San Jose", "product_id": "P093", "product_name": "Ultra Accessory", "product_category": "Jewelry", "quantity": 3, "price": 317.89, "total_amount": 953.67, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "17"}}
{"order_id": "3810", "order_date": "2023-07-30", "customer_id": "C808", "customer_name": "Ivy Garcia", "customer_gender": "male", "customer_age": 52, "customer_city": "Philadelphia", "product_id": "P013", "product_name": "Elite Gadget", "product_category": "Electronics", "quantity": 3, "price": 541.31, "total_amount": 1623.93, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "18"}}
{"order_id": "4772", "order_date": "2023-04-12", "customer_id": "C043", "customer_name": "David Garcia", "customer_gender": "male", "customer_age": 18, "customer_city": "Los Angeles", "product_id": "P009", "product_name": "Elite Gadget", "product_category": "Beauty", "quantity": 3, "price": 953.55, "total_amount": 2860.65, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "19"}}
{"order_id": "9792", "order_date": "2023-02-25", "customer_id": "C974", "customer_name": "Henry Miller", "customer_gender": "male", "customer_age": 37, "customer_city": "Los Angeles", "product_id": "P012", "product_name": "Ultra Device", "product_category": "Beauty", "quantity": 1, "price": 898.19, "total_amount": 898.19, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "20"}}
{"order_id": "8033", "order_date": "2023-02-17", "customer_id": "C280", "customer_name": "Bob Rodriguez", "customer_gender": "female", "customer_age": 63, "customer_city": "New York", "product_id": "P084", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 1, "price": 847.14, "total_amount": 847.14, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "21"}}
{"order_id": "4105", "order_date": "2023-11-30", "customer_id": "C605", "customer_name": "Henry Martinez", "customer_gender": "male", "customer_age": 24, "customer_city": "San Antonio", "product_id": "P003", "product_name": "Elite Widget", "product_category": "Groceries", "quantity": 3, "price": 871.75, "total_amount": 2615.25, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "22"}}
{"order_id": "5339", "order_date": "2023-05-25", "customer_id": "C323", "customer_name": "David Johnson", "customer_gender": "female", "customer_age": 41, "customer_city": "Dallas", "product_id": "P054", "product_name": "Elite Gadget", "product_category": "Fashion", "quantity": 4, "price": 501.88, "total_amount": 2007.52, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "23"}}
{"order_id": "2291", "order_date": "2023-02-22", "customer_id": "C180", "customer_name": "Bob Smith", "customer_gender": "male", "customer_age": 53, "customer_city": "Chicago", "product_id": "P065", "product_name": "Ultra Tool", "product_category": "Groceries", "quantity": 5, "price": 229.9, "total_amount": 1149.5, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "24"}}
{"order_id": "6857", "order_date": "2023-03-15", "customer_id": "C552", "customer_name": "Alice Smith", "customer_gender": "male", "customer_age": 21, "customer_city": "San Diego", "product_id": "P099", "product_name": "Elite Widget", "product_category": "Jewelry", "quantity": 2, "price": 621.51, "total_amount": 1243.02, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "25"}}
{"order_id": "9291", "order_date": "2023-07-12", "customer_id": "C424", "customer_name": "Grace Garcia", "customer_gender": "male", "customer_age": 63, "customer_city": "San Diego", "product_id": "P062", "product_name": "Super Device", "product_category": "Electronics", "quantity": 1, "price": 302.94, "total_amount": 302.94, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "26"}}
{"order_id": "1897", "order_date": "2023-01-20", "customer_id": "C056", "customer_name": "Grace Martinez", "customer_gender": "female", "customer_age": 40, "customer_city": "San Diego", "product_id": "P018", "product_name": "Mega Tool", "product_category": "Sports", "quantity": 5, "price": 635.06, "total_amount": 3175.3, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "27"}}
{"order_id": "5953", "order_date": "2023-04-14", "customer_id": "C363", "customer_name": "Henry Brown", "customer_gender": "female", "customer_age": 70, "customer_city": "New York", "product_id": "P024", "product_name": "Pro Widget", "product_category": "Home Appliances", "quantity": 2, "price": 882.01, "total_amount": 1764.02, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "28"}}
{"order_id": "3156", "order_date": "2023-03-12", "customer_id": "C878", "customer_name": "Henry Martinez", "customer_gender": "male", "customer_age": 22, "customer_city": "Houston", "product_id": "P087", "product_name": "Mega Accessory", "product_category": "Jewelry", "quantity": 1, "price": 535.02, "total_amount": 535.02, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "29"}}
{"order_id": "6906", "order_date": "2023-12-20", "customer_id": "C071", "customer_name": "Bob Martinez", "customer_gender": "male", "customer_age": 69, "customer_city": "Philadelphia", "product_id": "P034", "product_name": "Mega Accessory", "product_category": "Home Appliances", "quantity": 2, "price": 639.65, "total_amount": 1279.3, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "30"}}
{"order_id": "8191", "order_date": "2023-08-02", "customer_id": "C458", "customer_name": "Ivy Smith", "customer_gender": "female", "customer_age": 42, "customer_city": "Dallas", "product_id": "P016", "product_name": "Elite Tool", "product_category": "Books", "quantity": 2, "price": 366.21, "total_amount": 732.42, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "31"}}
{"order_id": "3182", "order_date": "2023-10-12", "customer_id": "C207", "customer_name": "Bob Garcia", "customer_gender": "female", "customer_age": 33, "customer_city": "San Diego", "product_id": "P047", "product_name": "Ultra Widget", "product_category": "Jewelry", "quantity": 3, "price": 833.95, "total_amount": 2501.85, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "32"}}
{"order_id": "4709", "order_date": "2023-05-21", "customer_id": "C451", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 40, "customer_city": "Chicago", "product_id": "P021", "product_name": "Mega Device", "product_category": "Groceries", "quantity": 3, "price": 106.01, "total_amount": 318.03, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "33"}}
{"order_id": "1550", "order_date": "2023-02-25", "customer_id": "C029", "customer_name": "Frank Smith", "customer_gender": "female", "customer_age": 44, "customer_city": "San Jose", "product_id": "P037", "product_name": "Elite Tool", "product_category": "Groceries", "quantity": 4, "price": 743.62, "total_amount": 2974.48, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "34"}}
{"order_id": "5735", "order_date": "2023-08-19", "customer_id": "C765", "customer_name": "David Davis", "customer_gender": "female", "customer_age": 42, "customer_city": "Philadelphia", "product_id": "P037", "product_name": "Super Accessory", "product_category": "Groceries", "quantity": 3, "price": 724.34, "total_amount": 2173.02, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "35"}}
{"order_id": "2189", "order_date": "2023-01-20", "customer_id": "C988", "customer_name": "Charlie Brown", "customer_gender": "male", "customer_age": 24, "customer_city": "San Diego", "product_id": "P004", "product_name": "Ultra Widget", "product_category": "Jewelry", "quantity": 1, "price": 206.12, "total_amount": 206.12, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "36"}}
{"order_id": "4261", "order_date": "2023-04-16", "customer_id": "C199", "customer_name": "Grace Brown", "customer_gender": "female", "customer_age": 30, "customer_city": "New York", "product_id": "P027", "product_name": "Mega Tool", "product_category": "Furniture", "quantity": 1, "price": 291.97, "total_amount": 291.97, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "37"}}
{"order_id": "3166", "order_date": "2023-03-21", "customer_id": "C949", "customer_name": "Ivy Rodriguez", "customer_gender": "female", "customer_age": 24, "customer_city": "Los Angeles", "product_id": "P008", "product_name": "Mega Widget", "product_category": "Books", "quantity": 1, "price": 600.69, "total_amount": 600.69, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "38"}}
{"order_id": "7539", "order_date": "2023-08-15", "customer_id": "C581", "customer_name": "Alice Williams", "customer_gender": "male", "customer_age": 69, "customer_city": "San Antonio", "product_id": "P010", "product_name": "Super Tool", "product_category": "Home Appliances", "quantity": 4, "price": 584.18, "total_amount": 2336.72, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "39"}}
{"order_id": "4588", "order_date": "2023-02-23", "customer_id": "C131", "customer_name": "Alice Smith", "customer_gender": "female", "customer_age": 62, "customer_city": "San Diego", "product_id": "P057", "product_name": "Elite Accessory", "product_category": "Electronics", "quantity": 5, "price": 553.25, "total_amount": 2766.25, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "40"}}
{"order_id": "4199", "order_date": "2023-11-10", "customer_id": "C494", "customer_name": "Charlie Jones", "customer_gender": "male", "customer_age": 45, "customer_city": "Houston", "product_id": "P020", "product_name": "Elite Tool", "product_category": "Beauty", "quantity": 1, "price": 342.63, "total_amount": 342.63, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "41"}}
{"order_id": "9196", "order_date": "2023-02-06", "customer_id": "C534", "customer_name": "Alice Garcia", "customer_gender": "female", "customer_age": 32, "customer_city": "Phoenix", "product_id": "P020", "product_name": "Mega Device", "product_category": "Furniture", "quantity": 1, "price": 635.12, "total_amount": 635.12, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "42"}}
{"order_id": "1941", "order_date": "2023-06-23", "customer_id": "C475", "customer_name": "Bob Rodriguez", "customer_gender": "female", "customer_age": 48, "customer_city": "Chicago", "product_id": "P090", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 4, "price": 215.28, "total_amount": 861.12, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "43"}}
{"order_id": "9818", "order_date": "2023-05-27", "customer_id": "C532", "customer_name": "Bob Johnson", "customer_gender": "male", "customer_age": 47, "customer_city": "Phoenix", "product_id": "P084", "product_name": "Super Tool", "product_category": "Home Appliances", "quantity": 3, "price": 827.32, "total_amount": 2481.96, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "44"}}
{"order_id": "1809", "order_date": "2023-07-13", "customer_id": "C317", "customer_name": "Charlie Miller", "customer_gender": "male", "customer_age": 36, "customer_city": "San Jose", "product_id": "P021", "product_name": "Pro Gadget", "product_category": "Jewelry", "quantity": 3, "price": 121.06, "total_amount": 363.18, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "45"}}
{"order_id": "1361", "order_date": "2023-01-29", "customer_id": "C138", "customer_name": "Charlie Davis", "customer_gender": "female", "customer_age": 50, "customer_city": "San Diego", "product_id": "P078", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 2, "price": 948.74, "total_amount": 1897.48, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "46"}}
{"order_id": "7019", "order_date": "2023-03-01", "customer_id": "C165", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 18, "customer_city": "Phoenix", "product_id": "P005", "product_name": "Elite Accessory", "product_category": "Furniture", "quantity": 5, "price": 963.32, "total_amount": 4816.6, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "47"}}
{"order_id": "2564", "order_date": "2023-12-31", "customer_id": "C983", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 34, "customer_city": "San Diego", "product_id": "P067", "product_name": "Super Gadget", "product_category": "Home Appliances", "quantity": 5, "price": 919.89, "total_amount": 4599.45, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "48"}}
{"order_id": "8300", "order_date": "2023-08-19", "customer_id": "C556", "customer_name": "Jack Martinez", "customer_gender": "female", "customer_age": 25, "customer_city": "Philadelphia", "product_id": "P069", "product_name": "Pro Tool", "product_category": "Toys", "quantity": 2, "price": 345.59, "total_amount": 691.18, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "49"}}
{"order_id": "1156", "order_date": "2023-06-15", "customer_id": "C438", "customer_name": "David Davis", "customer_gender": "male", "customer_age": 59, "customer_city": "Houston", "product_id": "P020", "product_name": "Mega Tool", "product_category": "Beauty", "quantity": 2, "price": 687.43, "total_amount": 1374.86, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "50"}}
{"order_id": "2897", "order_date": "2023-03-13", "customer_id": "C320", "customer_name": "Charlie Rodriguez", "customer_gender": "male", "customer_age": 68, "customer_city": "San Jose", "product_id": "P060", "product_name": "Pro Tool", "product_category": "Jewelry", "quantity": 5, "price": 918.59, "total_amount": 4592.95, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "51"}}
{"order_id": "5636", "order_date": "2023-05-12", "customer_id": "C643", "customer_name": "Jack Martinez", "customer_gender": "male", "customer_age": 33, "customer_city": "San Jose", "product_id": "P073", "product_name": "Ultra Tool", "product_category": "Fashion", "quantity": 4, "price": 141.53, "total_amount": 566.12, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "52"}}
{"order_id": "3983", "order_date": "2023-08-05", "customer_id": "C973", "customer_name": "Frank Johnson", "customer_gender": "female", "customer_age": 44, "customer_city": "San Jose", "product_id": "P084", "product_name": "Super Widget", "product_category": "Furniture", "quantity": 5, "price": 171.0, "total_amount": 855.0, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "53"}}
{"order_id": "5290", "order_date": "2023-01-02", "customer_id": "C369", "customer_name": "Jack Williams", "customer_gender": "male", "customer_age": 56, "customer_city": "Los Angeles", "product_id": "P095", "product_name": "Ultra Gadget", "product_category": "Jewelry", "quantity": 2, "price": 634.53, "total_amount": 1269.06, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "54"}}
{"order_id": "8195", "order_date": "2023-11-25", "customer_id": "C782", "customer_name": "Jack Martinez", "customer_gender": "male", "customer_age": 62, "customer_city": "Phoenix", "product_id": "P040", "product_name": "Ultra Gadget", "product_category": "Books", "quantity": 1, "price": 595.85, "total_amount": 595.85, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "55"}}
{"order_id": "6632", "order_date": "2023-03-01", "customer_id": "C596", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 23, "customer_city": "Philadelphia", "product_id": "P080", "product_name": "Super Tool", "product_category": "Fashion", "quantity": 1, "price": 641.04, "total_amount": 641.04, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "56"}}
{"order_id": "1364", "order_date": "2023-12-18", "customer_id": "C701", "customer_name": "Charlie Smith", "customer_gender": "male", "customer_age": 67, "customer_city": "Phoenix", "product_id": "P060", "product_name": "Pro Gadget", "product_category": "Fashion", "quantity": 1, "price": 978.14, "total_amount": 978.14, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "57"}}
{"order_id": "7109", "order_date": "2023-08-25", "customer_id": "C480", "customer_name": "Bob Jones", "customer_gender": "female", "customer_age": 43, "customer_city": "San Antonio", "product_id": "P010", "product_name": "Elite Widget", "product_category": "Fashion", "quantity": 1, "price": 29.28, "total_amount": 29.28, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "58"}}
{"order_id": "5865", "order_date": "2023-04-29", "customer_id": "C658", "customer_name": "Frank Garcia", "customer_gender": "male", "customer_age": 58, "customer_city": "Los Angeles", "product_id": "P004", "product_name": "Super Gadget", "product_category": "Groceries", "quantity": 3, "price": 798.19, "total_amount": 2394.57, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "59"}}
{"order_id": "2212", "order_date": "2023-10-09", "customer_id": "C863", "customer_name": "Henry Garcia", "customer_gender": "female", "customer_age": 50, "customer_city": "Chicago", "product_id": "P085", "product_name": "Mega Gadget", "product_category": "Home Appliances", "quantity": 2, "price": 291.02, "total_amount": 582.04, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "60"}}
{"order_id": "6114", "order_date": "2023-05-21", "customer_id": "C365", "customer_name": "Charlie Garcia", "customer_gender": "male", "customer_age": 36, "customer_city": "Los Angeles", "product_id": "P080", "product_name": "Super Accessory", "product_category": "Jewelry", "quantity": 1, "price": 254.71, "total_amount": 254.71, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "61"}}
{"order_id": "2867", "order_date": "2023-10-13", "customer_id": "C854", "customer_name": "Charlie Martinez", "customer_gender": "male", "customer_age": 21, "customer_city": "Houston", "product_id": "P031", "product_name": "Pro Widget", "product_category": "Beauty", "quantity": 4, "price": 679.3, "total_amount": 2717.2, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "62"}}
{"order_id": "5073", "order_date": "2023-12-25", "customer_id": "C285", "customer_name": "Bob Smith", "customer_gender": "male", "customer_age": 29, "customer_city": "San Diego", "product_id": "P078", "product_name": "Elite Accessory", "product_category": "Home Appliances", "quantity": 3, "price": 473.43, "total_amount": 1420.29, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "63"}}
{"order_id": "9574", "order_date": "2023-06-26", "customer_id": "C023", "customer_name": "Charlie Garcia", "customer_gender": "female", "customer_age": 40, "customer_city": "San Jose", "product_id": "P076", "product_name": "Mega Widget", "product_category": "Furniture", "quantity": 2, "price": 115.01, "total_amount": 230.02, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "64"}}
{"order_id": "5086", "order_date": "2023-01-28", "customer_id": "C965", "customer_name": "Jack Jones", "customer_gender": "female", "customer_age": 33, "customer_city": "Philadelphia", "product_id": "P036", "product_name": "Ultra Device", "product_category": "Fashion", "quantity": 4, "price": 326.84, "total_amount": 1307.36, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "65"}}
{"order_id": "2320", "order_date": "2023-11-17", "customer_id": "C789", "customer_name": "David Smith", "customer_gender": "male", "customer_age": 21, "customer_city": "Chicago", "product_id": "P041", "product_name": "Elite Accessory", "product_category": "Sports", "quantity": 5, "price": 498.44, "total_amount": 2492.2, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "66"}}
{"order_id": "2463", "order_date": "2023-12-07", "customer_id": "C038", "customer_name": "Jack Jones", "customer_gender": "male", "customer_age": 66, "customer_city": "Dallas", "product_id": "P017", "product_name": "Super Device", "product_category": "Beauty", "quantity": 3, "price": 499.37, "total_amount": 1498.11, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "67"}}
{"order_id": "9420", "order_date": "2023-06-25", "customer_id": "C336", "customer_name": "Grace Brown", "customer_gender": "female", "customer_age": 24, "customer_city": "Los Angeles", "product_id": "P035", "product_name": "Elite Device", "product_category": "Fashion", "quantity": 3, "price": 70.95, "total_amount": 212.85, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "68"}}
{"order_id": "1322", "order_date": "2023-01-27", "customer_id": "C457", "customer_name": "Jack Garcia", "customer_gender": "male", "customer_age": 19, "customer_city": "Los Angeles", "product_id": "P076", "product_name": "Ultra Tool", "product_category": "Beauty", "quantity": 5, "price": 742.85, "total_amount": 3714.25, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "69"}}
{"order_id": "5018", "order_date": "2023-07-10", "customer_id": "C409", "customer_name": "Frank Martinez", "customer_gender": "male", "customer_age": 47, "customer_city": "San Jose", "product_id": "P021", "product_name": "Ultra Gadget", "product_category": "Fashion", "quantity": 5, "price": 266.16, "total_amount": 1330.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "70"}}
{"order_id": "7009", "order_date": "2023-05-10", "customer_id": "C923", "customer_name": "Jack Williams", "customer_gender": "male", "customer_age": 62, "customer_city": "Dallas", "product_id": "P001", "product_name": "Ultra Tool", "product_category": "Books", "quantity": 1, "price": 811.29, "total_amount": 811.29, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "71"}}
{"order_id": "5884", "order_date": "2023-05-29", "customer_id": "C528", "customer_name": "Charlie Johnson", "customer_gender": "female", "customer_age": 18, "customer_city": "Philadelphia", "product_id": "P031", "product_name": "Elite Gadget", "product_category": "Books", "quantity": 1, "price": 379.51, "total_amount": 379.51, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "72"}}
{"order_id": "6635", "order_date": "2023-01-15", "customer_id": "C575", "customer_name": "Frank Brown", "customer_gender": "male", "customer_age": 51, "customer_city": "New York", "product_id": "P089", "product_name": "Elite Accessory", "product_category": "Electronics", "quantity": 4, "price": 781.98, "total_amount": 3127.92, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "73"}}
{"order_id": "9082", "order_date": "2023-10-10", "customer_id": "C116", "customer_name": "Ivy Brown", "customer_gender": "male", "customer_age": 37, "customer_city": "Houston", "product_id": "P008", "product_name": "Elite Widget", "product_category": "Sports", "quantity": 4, "price": 111.95, "total_amount": 447.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "74"}}
{"order_id": "8042", "order_date": "2023-11-25", "customer_id": "C041", "customer_name": "Bob Davis", "customer_gender": "female", "customer_age": 67, "customer_city": "Chicago", "product_id": "P062", "product_name": "Ultra Accessory", "product_category": "Books", "quantity": 2, "price": 352.53, "total_amount": 705.06, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "75"}}
{"order_id": "4047", "order_date": "2023-01-19", "customer_id": "C200", "customer_name": "Ivy Miller", "customer_gender": "female", "customer_age": 27, "customer_city": "San Diego", "product_id": "P056", "product_name": "Pro Device", "product_category": "Electronics", "quantity": 2, "price": 298.64, "total_amount": 597.28, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "76"}}
{"order_id": "4072", "order_date": "2023-12-01", "customer_id": "C126", "customer_name": "Grace Williams", "customer_gender": "female", "customer_age": 31, "customer_city": "Houston", "product_id": "P083", "product_name": "Pro Gadget", "product_category": "Groceries", "quantity": 2, "price": 480.09, "total_amount": 960.18, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "77"}}
{"order_id": "7911", "order_date": "2023-12-31", "customer_id": "C503", "customer_name": "Bob Jones", "customer_gender": "female", "customer_age": 21, "customer_city": "Los Angeles", "product_id": "P005", "product_name": "Elite Gadget", "product_category": "Electronics", "quantity": 3, "price": 192.63, "total_amount": 577.89, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "78"}}
{"order_id": "3590", "order_date": "2023-06-30", "customer_id": "C003", "customer_name": "Henry Smith", "customer_gender": "male", "customer_age": 23, "customer_city": "San Jose", "product_id": "P099", "product_name": "Pro Widget", "product_category": "Home Appliances", "quantity": 4, "price": 46.63, "total_amount": 186.52, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "79"}}
{"order_id": "4306", "order_date": "2023-07-09", "customer_id": "C589", "customer_name": "Bob Rodriguez", "customer_gender": "male", "customer_age": 42, "customer_city": "Philadelphia", "product_id": "P051", "product_name": "Ultra Accessory", "product_category": "Beauty", "quantity": 3, "price": 570.6, "total_amount": 1711.8, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "80"}}
{"order_id": "4255", "order_date": "2023-02-02", "customer_id": "C003", "customer_name": "Henry Miller", "customer_gender": "male", "customer_age": 66, "customer_city": "San Antonio", "product_id": "P076", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 4, "price": 901.45, "total_amount": 3605.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "81"}}
{"order_id": "1162", "order_date": "2023-12-25", "customer_id": "C018", "customer_name": "Grace Smith", "customer_gender": "female", "customer_age": 63, "customer_city": "New York", "product_id": "P067", "product_name": "Ultra Device", "product_category": "Groceries", "quantity": 4, "price": 155.11, "total_amount": 620.44, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "82"}}
{"order_id": "9635", "order_date": "2023-05-12", "customer_id": "C237", "customer_name": "Henry Johnson", "customer_gender": "female", "customer_age": 61, "customer_city": "New York", "product_id": "P079", "product_name": "Ultra Widget", "product_category": "Beauty", "quantity": 2, "price": 773.0, "total_amount": 1546.0, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "83"}}
{"order_id": "6795", "order_date": "2023-06-14", "customer_id": "C336", "customer_name": "Ivy Jones", "customer_gender": "male", "customer_age": 54, "customer_city": "Philadelphia", "product_id": "P010", "product_name": "Mega Device", "product_category": "Electronics", "quantity": 4, "price": 394.85, "total_amount": 1579.4, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "84"}}
{"order_id": "6288", "order_date": "2023-09-22", "customer_id": "C327", "customer_name": "Jack Smith", "customer_gender": "male", "customer_age": 68, "customer_city": "Houston", "product_id": "P072", "product_name": "Ultra Tool", "product_category": "Home Appliances", "quantity": 3, "price": 919.6, "total_amount": 2758.8, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "85"}}
{"order_id": "5127", "order_date": "2023-01-13", "customer_id": "C243", "customer_name": "David Williams", "customer_gender": "female", "customer_age": 69, "customer_city": "Philadelphia", "product_id": "P043", "product_name": "Elite Device", "product_category": "Home Appliances", "quantity": 3, "price": 954.01, "total_amount": 2862.03, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "86"}}
{"order_id": "2322", "order_date": "2023-10-03", "customer_id": "C543", "customer_name": "Frank Miller", "customer_gender": "male", "customer_age": 49, "customer_city": "Los Angeles", "product_id": "P099", "product_name": "Super Widget", "product_category": "Beauty", "quantity": 5, "price": 971.79, "total_amount": 4858.95, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "87"}}
{"order_id": "9268", "order_date": "2023-01-07", "customer_id": "C136", "customer_name": "Henry Rodriguez", "customer_gender": "male", "customer_age": 63, "customer_city": "Houston", "product_id": "P046", "product_name": "Super Widget", "product_category": "Furniture", "quantity": 3, "price": 896.47, "total_amount": 2689.41, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "88"}}
{"order_id": "7091", "order_date": "2023-05-04", "customer_id": "C551", "customer_name": "Grace Brown", "customer_gender": "male", "customer_age": 61, "customer_city": "Chicago", "product_id": "P016", "product_name": "Ultra Gadget", "product_category": "Toys", "quantity": 4, "price": 95.13, "total_amount": 380.52, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "89"}}
{"order_id": "2123", "order_date": "2023-10-05", "customer_id": "C483", "customer_name": "Henry Rodriguez", "customer_gender": "male", "customer_age": 55, "customer_city": "San Antonio", "product_id": "P006", "product_name": "Mega Device", "product_category": "Jewelry", "quantity": 2, "price": 499.08, "total_amount": 998.16, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "90"}}
{"order_id": "8013", "order_date": "2023-04-29", "customer_id": "C431", "customer_name": "Eva Rodriguez", "customer_gender": "male", "customer_age": 65, "customer_city": "Phoenix", "product_id": "P046", "product_name": "Elite Device", "product_category": "Furniture", "quantity": 4, "price": 889.3, "total_amount": 3557.2, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "91"}}
{"order_id": "1329", "order_date": "2023-02-27", "customer_id": "C198", "customer_name": "David Johnson", "customer_gender": "male", "customer_age": 64, "customer_city": "Dallas", "product_id": "P001", "product_name": "Pro Tool", "product_category": "Electronics", "quantity": 1, "price": 734.57, "total_amount": 734.57, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "92"}}
{"order_id": "1937", "order_date": "2023-06-27", "customer_id": "C928", "customer_name": "Jack Garcia", "customer_gender": "female", "customer_age": 27, "customer_city": "Los Angeles", "product_id": "P002", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 3, "price": 241.4, "total_amount": 724.2, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "93"}}
{"order_id": "1472", "order_date": "2023-03-17", "customer_id": "C365", "customer_name": "Frank Miller", "customer_gender": "male", "customer_age": 40, "customer_city": "Los Angeles", "product_id": "P077", "product_name": "Elite Tool", "product_category": "Furniture", "quantity": 4, "price": 150.69, "total_amount": 602.76, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "94"}}
{"order_id": "5187", "order_date": "2023-06-07", "customer_id": "C725", "customer_name": "Ivy Martinez", "customer_gender": "female", "customer_age": 49, "customer_city": "Dallas", "product_id": "P025", "product_name": "Super Accessory", "product_category": "Jewelry", "quantity": 1, "price": 904.01, "total_amount": 904.01, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "95"}}
{"order_id": "1737", "order_date": "2023-09-15", "customer_id": "C977", "customer_name": "Ivy Brown", "customer_gender": "female", "customer_age": 40, "customer_city": "New York", "product_id": "P031", "product_name": "Elite Widget", "product_category": "Sports", "quantity": 5, "price": 977.22, "total_amount": 4886.1, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "96"}}
{"order_id": "3054", "order_date": "2023-02-16", "customer_id": "C872", "customer_name": "David Miller", "customer_gender": "female", "customer_age": 52, "customer_city": "San Jose", "product_id": "P077", "product_name": "Ultra Gadget", "product_category": "Jewelry", "quantity": 5, "price": 493.64, "total_amount": 2468.2, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "97"}}
{"order_id": "5606", "order_date": "2023-06-12", "customer_id": "C053", "customer_name": "Charlie Williams", "customer_gender": "female", "customer_age": 55, "customer_city": "San Antonio", "product_id": "P027", "product_name": "Elite Device", "product_category": "Jewelry", "quantity": 1, "price": 698.18, "total_amount": 698.18, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "98"}}
{"order_id": "6873", "order_date": "2023-12-19", "customer_id": "C209", "customer_name": "Henry Davis", "customer_gender": "male", "customer_age": 63, "customer_city": "New York", "product_id": "P012", "product_name": "Super Gadget", "product_category": "Electronics", "quantity": 2, "price": 287.91, "total_amount": 575.82, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "99"}}
{"order_id": "2082", "order_date": "2023-02-27", "customer_id": "C184", "customer_name": "Charlie Rodriguez", "customer_gender": "male", "customer_age": 45, "customer_city": "San Jose", "product_id": "P043", "product_name": "Super Tool", "product_category": "Toys", "quantity": 4, "price": 621.0, "total_amount": 2484.0, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "100"}}
{"order_id": "1211", "order_date": "2023-04-11", "customer_id": "C549", "customer_name": "Alice Garcia", "customer_gender": "male", "customer_age": 62, "customer_city": "Houston", "product_id": "P047", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 5, "price": 758.18, "total_amount": 3790.9, "payment_method": "PayPal", "is_returned": false}
```

### 结果--
```
{
  "errors": false,
  "took": 15,
  "items": [
    {
      "index": {
        "_index": "ecommerce",
        "_id": "1",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 100,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "2",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 101,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "3",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 102,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "4",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 103,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "5",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 104,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "6",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 105,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "7",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 106,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "8",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 107,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "9",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 108,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "10",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 109,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "11",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 110,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "12",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 111,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "13",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 112,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "14",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 113,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "15",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 114,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "16",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 115,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "17",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 116,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "18",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 117,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "19",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 118,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "20",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 119,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "21",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 120,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "22",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 121,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "23",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 122,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "24",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 123,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "25",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 124,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "26",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 125,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "27",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 126,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "28",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 127,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "29",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 128,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "30",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 129,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "31",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 130,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "32",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 131,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "33",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 132,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "34",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 133,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "35",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 134,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "36",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 135,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "37",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 136,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "38",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 137,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "39",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 138,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "40",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 139,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "41",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 140,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "42",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 141,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "43",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 142,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "44",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 143,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "45",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 144,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "46",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 145,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "47",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 146,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "48",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 147,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "49",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 148,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "50",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 149,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "51",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 150,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "52",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 151,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "53",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 152,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "54",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 153,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "55",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 154,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "56",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 155,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "57",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 156,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "58",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 157,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "59",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 158,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "60",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 159,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "61",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 160,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "62",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 161,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "63",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 162,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "64",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 163,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "65",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 164,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "66",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 165,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "67",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 166,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "68",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 167,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "69",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 168,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "70",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 169,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "71",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 170,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "72",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 171,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "73",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 172,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "74",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 173,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "75",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 174,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "76",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 175,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "77",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 176,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "78",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 177,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "79",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 178,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "80",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 179,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "81",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 180,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "82",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 181,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "83",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 182,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "84",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 183,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "85",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 184,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "86",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 185,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "87",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 186,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "88",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 187,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "89",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 188,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "90",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 189,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "91",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 190,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "92",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 191,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "93",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 192,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "94",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 193,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "95",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 194,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "96",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 195,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "97",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 196,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "98",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 197,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "99",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 198,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "ecommerce",
        "_id": "100",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 199,
        "_primary_term": 1,
        "status": 200
      }
    }
  ]
}
```
### 示例图片--
![Markdown Logo](./Images/2.png)

### 数据分析--1.统计每个产品类别的总销售额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "sales_by_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

### 结果--
```
{
  "took": 5,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "sales_by_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "total_sales": {
            "value": 17477.37028503418
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "total_sales": {
            "value": 18941.559997558594
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "total_sales": {
            "value": 25865.190231323242
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "total_sales": {
            "value": 22708.94012451172
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "total_sales": {
            "value": 17228.31996154785
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "total_sales": {
            "value": 11878.820098876953
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "total_sales": {
            "value": 16172.980072021484
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "total_sales": {
            "value": 7073.110048294067
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "total_sales": {
            "value": 10528.830047607422
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "total_sales": {
            "value": 13250.500122070312
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/3.png)
### 数据分析--2.计算每个城市的平均订单金额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_order_by_city": {
      "terms": {
        "field": "customer_city"
      },
      "aggs": {
        "average_order": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_order_by_city": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Los Angeles",
          "doc_count": 13,
          "average_order": {
            "value": 1482.7977142333984
          }
        },
        {
          "key": "San Diego",
          "doc_count": 13,
          "average_order": {
            "value": 1878.4761915940505
          }
        },
        {
          "key": "San Jose",
          "doc_count": 12,
          "average_order": {
            "value": 1587.2216771443684
          }
        },
        {
          "key": "Philadelphia",
          "doc_count": 11,
          "average_order": {
            "value": 1370.19365345348
          }
        },
        {
          "key": "New York",
          "doc_count": 10,
          "average_order": {
            "value": 1801.9510040283203
          }
        },
        {
          "key": "Chicago",
          "doc_count": 9,
          "average_order": {
            "value": 1062.1610989040798
          }
        },
        {
          "key": "Houston",
          "doc_count": 9,
          "average_order": {
            "value": 1735.199978298611
          }
        },
        {
          "key": "Dallas",
          "doc_count": 8,
          "average_order": {
            "value": 1291.8024978637695
          }
        },
        {
          "key": "Phoenix",
          "doc_count": 8,
          "average_order": {
            "value": 2315.7625274658203
          }
        },
        {
          "key": "San Antonio",
          "doc_count": 7,
          "average_order": {
            "value": 1607.7128516605922
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/4.png)
### 数据分析--3.找出销量最高的前5个产品
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_selling_products": {
      "terms": {
        "field": "product_name",
        "size": 5,
        "order": {
          "total_quantity": "desc"
        }
      },
      "aggs": {
        "total_quantity": {
          "sum": {
            "field": "quantity"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_selling_products": {
      "doc_count_error_upper_bound": -1,
      "sum_other_doc_count": 67,
      "buckets": [
        {
          "key": "Elite Accessory",
          "doc_count": 6,
          "total_quantity": {
            "value": 25
          }
        },
        {
          "key": "Ultra Device",
          "doc_count": 7,
          "total_quantity": {
            "value": 24
          }
        },
        {
          "key": "Ultra Accessory",
          "doc_count": 8,
          "total_quantity": {
            "value": 22
          }
        },
        {
          "key": "Ultra Gadget",
          "doc_count": 6,
          "total_quantity": {
            "value": 21
          }
        },
        {
          "key": "Ultra Tool",
          "doc_count": 6,
          "total_quantity": {
            "value": 21
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/5.png)
### 数据分析--4.计算男性和女性客户的平均年龄
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "avg_age_by_gender": {
      "terms": {
        "field": "customer_gender"
      },
      "aggs": {
        "average_age": {
          "avg": {
            "field": "customer_age"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "avg_age_by_gender": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "male",
          "doc_count": 55,
          "average_age": {
            "value": 45.61818181818182
          }
        },
        {
          "key": "female",
          "doc_count": 45,
          "average_age": {
            "value": 43.888888888888886
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/6.png)
### 数据分析--5.统计每种支付方式的使用次数和总金额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "payment_stats": {
      "terms": {
        "field": "payment_method"
      },
      "aggs": {
        "total_amount": {
          "sum": {
            "field": "total_amount"
          }
        },
        "usage_count": {
          "value_count": {
            "field": "payment_method"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "payment_stats": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Cash on Delivery",
          "doc_count": 29,
          "usage_count": {
            "value": 29
          },
          "total_amount": {
            "value": 38746.55044555664
          }
        },
        {
          "key": "Debit Card",
          "doc_count": 29,
          "usage_count": {
            "value": 29
          },
          "total_amount": {
            "value": 48975.19040107727
          }
        },
        {
          "key": "Credit Card",
          "doc_count": 22,
          "usage_count": {
            "value": 22
          },
          "total_amount": {
            "value": 36358.470153808594
          }
        },
        {
          "key": "PayPal",
          "doc_count": 20,
          "usage_count": {
            "value": 20
          },
          "total_amount": {
            "value": 37045.40998840332
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/7.png)
### 数据分析--6.计算每月的总销售额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "monthly_sales": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "month"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 5,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "monthly_sales": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 12,
          "total_sales": {
            "value": 24381.61993408203
          }
        },
        {
          "key_as_string": "2023-02-01T00:00:00.000Z",
          "key": 1675209600000,
          "doc_count": 11,
          "total_sales": {
            "value": 18870.850006103516
          }
        },
        {
          "key_as_string": "2023-03-01T00:00:00.000Z",
          "key": 1677628800000,
          "doc_count": 10,
          "total_sales": {
            "value": 17959.33026123047
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 9,
          "total_sales": {
            "value": 18775.959869384766
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 10,
          "total_sales": {
            "value": 11713.169967651367
          }
        },
        {
          "key_as_string": "2023-06-01T00:00:00.000Z",
          "key": 1685577600000,
          "doc_count": 9,
          "total_sales": {
            "value": 6771.1600341796875
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 7,
          "total_sales": {
            "value": 9110.010131835938
          }
        },
        {
          "key_as_string": "2023-08-01T00:00:00.000Z",
          "key": 1690848000000,
          "doc_count": 8,
          "total_sales": {
            "value": 12135.870210647583
          }
        },
        {
          "key_as_string": "2023-09-01T00:00:00.000Z",
          "key": 1693526400000,
          "doc_count": 3,
          "total_sales": {
            "value": 8615.500122070312
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 6,
          "total_sales": {
            "value": 12106.000183105469
          }
        },
        {
          "key_as_string": "2023-11-01T00:00:00.000Z",
          "key": 1698796800000,
          "doc_count": 6,
          "total_sales": {
            "value": 8176.529968261719
          }
        },
        {
          "key_as_string": "2023-12-01T00:00:00.000Z",
          "key": 1701388800000,
          "doc_count": 9,
          "total_sales": {
            "value": 12509.620300292969
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/8.png)
### 数据分析--7.找出平均订单金额最高的前3个客户
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_customers": {
      "terms": {
        "field": "customer_id",
        "size": 3,
        "order": {
          "avg_order_value": "desc"
        }
      },
      "aggs": {
        "avg_order_value": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 6,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_customers": {
      "doc_count_error_upper_bound": -1,
      "sum_other_doc_count": 97,
      "buckets": [
        {
          "key": "C977",
          "doc_count": 1,
          "avg_order_value": {
            "value": 4886.10009765625
          }
        },
        {
          "key": "C543",
          "doc_count": 1,
          "avg_order_value": {
            "value": 4858.9501953125
          }
        },
        {
          "key": "C165",
          "doc_count": 1,
          "avg_order_value": {
            "value": 4816.60009765625
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/9.png)
### 数据分析--8.计算每个年龄段（18-30，31-50，51+）的客户数量
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "age_ranges": {
      "range": {
        "field": "customer_age",
        "ranges": [
          { "from": 18, "to": 30 },
          { "from": 31, "to": 50 },
          { "from": 51 }
        ]
      }
    }
  }
}

```

### 结果--
```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "age_ranges": {
      "buckets": [
        {
          "key": "18.0-30.0",
          "from": 18,
          "to": 30,
          "doc_count": 24
        },
        {
          "key": "31.0-50.0",
          "from": 31,
          "to": 50,
          "doc_count": 31
        },
        {
          "key": "51.0-*",
          "from": 51,
          "doc_count": 42
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/10.png)
### 数据分析--9.计算每个产品类别的平均单价
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "avg_price_by_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "average_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "avg_price_by_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "average_price": {
            "value": 537.6807218279157
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "average_price": {
            "value": 484.44461763822113
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "average_price": {
            "value": 645.6961549612192
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "average_price": {
            "value": 701.0781749378551
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "average_price": {
            "value": 518.5519981384277
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "average_price": {
            "value": 524.0977762010363
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "average_price": {
            "value": 566.4644444783529
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "average_price": {
            "value": 369.4774992465973
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "average_price": {
            "value": 485.97999899727955
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "average_price": {
            "value": 483.9566599527995
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/11.png)
### 数据分析--10.找出订单数量最多的前5个城市
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_cities": {
      "terms": {
        "field": "customer_city",
        "size": 5,
        "order": {
          "_count": "desc"
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_cities": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 41,
      "buckets": [
        {
          "key": "Los Angeles",
          "doc_count": 13
        },
        {
          "key": "San Diego",
          "doc_count": 13
        },
        {
          "key": "San Jose",
          "doc_count": 12
        },
        {
          "key": "Philadelphia",
          "doc_count": 11
        },
        {
          "key": "New York",
          "doc_count": 10
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/12.png)
### 数据分析--11.计算每个季度的平均订单金额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "quarterly_sales": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "avg_order_value": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "quarterly_sales": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 33,
          "avg_order_value": {
            "value": 1854.9030364065459
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 28,
          "avg_order_value": {
            "value": 1330.724638257708
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 18,
          "avg_order_value": {
            "value": 1658.9655813641018
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 21,
          "avg_order_value": {
            "value": 1561.530973888579
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/13.png)
### 数据分析--12.统计每个产品类别中的商品数量
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "products_by_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "product_count": {
          "value_count": {
            "field": "product_id"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "products_by_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "product_count": {
            "value": 14
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "product_count": {
            "value": 13
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "product_count": {
            "value": 13
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "product_count": {
            "value": 11
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "product_count": {
            "value": 10
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "product_count": {
            "value": 9
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "product_count": {
            "value": 9
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "product_count": {
            "value": 8
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "product_count": {
            "value": 7
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "product_count": {
            "value": 6
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/14.png)
### 数据分析--13.计算男性和女性客户的平均订单金额
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "avg_order_by_gender": {
      "terms": {
        "field": "customer_gender"
      },
      "aggs": {
        "average_order": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "avg_order_by_gender": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "male",
          "doc_count": 55,
          "average_order": {
            "value": 1798.5043728915127
          }
        },
        {
          "key": "female",
          "doc_count": 45,
          "average_order": {
            "value": 1382.397343995836
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/15.png)
### 数据分析--14.找出总销售额最高的前10个日期
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_sales_dates": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day",
        "order": {
          "total_sales": "desc"
        }
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 4,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_sales_dates": {
      "buckets": [
        {
          "key_as_string": "2023-04-29T00:00:00.000Z",
          "key": 1682726400000,
          "doc_count": 2,
          "total_sales": {
            "value": 5951.77001953125
          }
        },
        {
          "key_as_string": "2023-03-01T00:00:00.000Z",
          "key": 1677628800000,
          "doc_count": 2,
          "total_sales": {
            "value": 5457.640075683594
          }
        },
        {
          "key_as_string": "2023-12-31T00:00:00.000Z",
          "key": 1703980800000,
          "doc_count": 2,
          "total_sales": {
            "value": 5177.3402099609375
          }
        },
        {
          "key_as_string": "2023-09-15T00:00:00.000Z",
          "key": 1694736000000,
          "doc_count": 1,
          "total_sales": {
            "value": 4886.10009765625
          }
        },
        {
          "key_as_string": "2023-10-03T00:00:00.000Z",
          "key": 1696291200000,
          "doc_count": 1,
          "total_sales": {
            "value": 4858.9501953125
          }
        },
        {
          "key_as_string": "2023-03-13T00:00:00.000Z",
          "key": 1678665600000,
          "doc_count": 1,
          "total_sales": {
            "value": 4592.9501953125
          }
        },
        {
          "key_as_string": "2023-08-04T00:00:00.000Z",
          "key": 1691107200000,
          "doc_count": 1,
          "total_sales": {
            "value": 4131.9501953125
          }
        },
        {
          "key_as_string": "2023-02-25T00:00:00.000Z",
          "key": 1677283200000,
          "doc_count": 2,
          "total_sales": {
            "value": 3872.6699829101562
          }
        },
        {
          "key_as_string": "2023-04-11T00:00:00.000Z",
          "key": 1681171200000,
          "doc_count": 1,
          "total_sales": {
            "value": 3790.89990234375
          }
        },
        {
          "key_as_string": "2023-01-27T00:00:00.000Z",
          "key": 1674777600000,
          "doc_count": 1,
          "total_sales": {
            "value": 3714.25
          }
        },
        {
          "key_as_string": "2023-03-28T00:00:00.000Z",
          "key": 1679961600000,
          "doc_count": 1,
          "total_sales": {
            "value": 3614.449951171875
          }
        },
        {
          "key_as_string": "2023-02-02T00:00:00.000Z",
          "key": 1675296000000,
          "doc_count": 1,
          "total_sales": {
            "value": 3605.800048828125
          }
        },
        {
          "key_as_string": "2023-08-15T00:00:00.000Z",
          "key": 1692057600000,
          "doc_count": 2,
          "total_sales": {
            "value": 3523.02001953125
          }
        },
        {
          "key_as_string": "2023-01-20T00:00:00.000Z",
          "key": 1674172800000,
          "doc_count": 2,
          "total_sales": {
            "value": 3381.4200439453125
          }
        },
        {
          "key_as_string": "2023-02-27T00:00:00.000Z",
          "key": 1677456000000,
          "doc_count": 2,
          "total_sales": {
            "value": 3218.5700073242188
          }
        },
        {
          "key_as_string": "2023-01-15T00:00:00.000Z",
          "key": 1673740800000,
          "doc_count": 1,
          "total_sales": {
            "value": 3127.919921875
          }
        },
        {
          "key_as_string": "2023-05-02T00:00:00.000Z",
          "key": 1682985600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2967.510009765625
          }
        },
        {
          "key_as_string": "2023-08-19T00:00:00.000Z",
          "key": 1692403200000,
          "doc_count": 2,
          "total_sales": {
            "value": 2864.2000122070312
          }
        },
        {
          "key_as_string": "2023-01-13T00:00:00.000Z",
          "key": 1673568000000,
          "doc_count": 1,
          "total_sales": {
            "value": 2862.030029296875
          }
        },
        {
          "key_as_string": "2023-04-12T00:00:00.000Z",
          "key": 1681257600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2860.64990234375
          }
        },
        {
          "key_as_string": "2023-02-23T00:00:00.000Z",
          "key": 1677110400000,
          "doc_count": 1,
          "total_sales": {
            "value": 2766.25
          }
        },
        {
          "key_as_string": "2023-09-22T00:00:00.000Z",
          "key": 1695340800000,
          "doc_count": 1,
          "total_sales": {
            "value": 2758.800048828125
          }
        },
        {
          "key_as_string": "2023-10-13T00:00:00.000Z",
          "key": 1697155200000,
          "doc_count": 1,
          "total_sales": {
            "value": 2717.199951171875
          }
        },
        {
          "key_as_string": "2023-01-07T00:00:00.000Z",
          "key": 1673049600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2689.409912109375
          }
        },
        {
          "key_as_string": "2023-11-30T00:00:00.000Z",
          "key": 1701302400000,
          "doc_count": 1,
          "total_sales": {
            "value": 2615.25
          }
        },
        {
          "key_as_string": "2023-01-10T00:00:00.000Z",
          "key": 1673308800000,
          "doc_count": 1,
          "total_sales": {
            "value": 2581.739990234375
          }
        },
        {
          "key_as_string": "2023-10-12T00:00:00.000Z",
          "key": 1697068800000,
          "doc_count": 1,
          "total_sales": {
            "value": 2501.85009765625
          }
        },
        {
          "key_as_string": "2023-11-17T00:00:00.000Z",
          "key": 1700179200000,
          "doc_count": 1,
          "total_sales": {
            "value": 2492.199951171875
          }
        },
        {
          "key_as_string": "2023-05-27T00:00:00.000Z",
          "key": 1685145600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2481.9599609375
          }
        },
        {
          "key_as_string": "2023-02-16T00:00:00.000Z",
          "key": 1676505600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2468.199951171875
          }
        },
        {
          "key_as_string": "2023-07-22T00:00:00.000Z",
          "key": 1689984000000,
          "doc_count": 1,
          "total_sales": {
            "value": 2220.9599609375
          }
        },
        {
          "key_as_string": "2023-05-12T00:00:00.000Z",
          "key": 1683849600000,
          "doc_count": 2,
          "total_sales": {
            "value": 2112.1199951171875
          }
        },
        {
          "key_as_string": "2023-04-27T00:00:00.000Z",
          "key": 1682553600000,
          "doc_count": 1,
          "total_sales": {
            "value": 2041.719970703125
          }
        },
        {
          "key_as_string": "2023-12-25T00:00:00.000Z",
          "key": 1703462400000,
          "doc_count": 2,
          "total_sales": {
            "value": 2040.7300415039062
          }
        },
        {
          "key_as_string": "2023-05-25T00:00:00.000Z",
          "key": 1684972800000,
          "doc_count": 1,
          "total_sales": {
            "value": 2007.52001953125
          }
        },
        {
          "key_as_string": "2023-01-29T00:00:00.000Z",
          "key": 1674950400000,
          "doc_count": 1,
          "total_sales": {
            "value": 1897.47998046875
          }
        },
        {
          "key_as_string": "2023-04-14T00:00:00.000Z",
          "key": 1681430400000,
          "doc_count": 1,
          "total_sales": {
            "value": 1764.02001953125
          }
        },
        {
          "key_as_string": "2023-07-09T00:00:00.000Z",
          "key": 1688860800000,
          "doc_count": 1,
          "total_sales": {
            "value": 1711.800048828125
          }
        },
        {
          "key_as_string": "2023-07-30T00:00:00.000Z",
          "key": 1690675200000,
          "doc_count": 1,
          "total_sales": {
            "value": 1623.9300537109375
          }
        },
        {
          "key_as_string": "2023-06-14T00:00:00.000Z",
          "key": 1686700800000,
          "doc_count": 1,
          "total_sales": {
            "value": 1579.4000244140625
          }
        },
        {
          "key_as_string": "2023-07-06T00:00:00.000Z",
          "key": 1688601600000,
          "doc_count": 1,
          "total_sales": {
            "value": 1556.4000244140625
          }
        },
        {
          "key_as_string": "2023-12-07T00:00:00.000Z",
          "key": 1701907200000,
          "doc_count": 1,
          "total_sales": {
            "value": 1498.1099853515625
          }
        },
        {
          "key_as_string": "2023-11-14T00:00:00.000Z",
          "key": 1699920000000,
          "doc_count": 1,
          "total_sales": {
            "value": 1425.5400390625
          }
        },
        {
          "key_as_string": "2023-06-15T00:00:00.000Z",
          "key": 1686787200000,
          "doc_count": 1,
          "total_sales": {
            "value": 1374.8599853515625
          }
        },
        {
          "key_as_string": "2023-07-10T00:00:00.000Z",
          "key": 1688947200000,
          "doc_count": 1,
          "total_sales": {
            "value": 1330.800048828125
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 1,
          "total_sales": {
            "value": 1329.280029296875
          }
        },
        {
          "key_as_string": "2023-01-28T00:00:00.000Z",
          "key": 1674864000000,
          "doc_count": 1,
          "total_sales": {
            "value": 1307.3599853515625
          }
        },
        {
          "key_as_string": "2023-11-25T00:00:00.000Z",
          "key": 1700870400000,
          "doc_count": 2,
          "total_sales": {
            "value": 1300.9099731445312
          }
        },
        {
          "key_as_string": "2023-12-20T00:00:00.000Z",
          "key": 1703030400000,
          "doc_count": 1,
          "total_sales": {
            "value": 1279.300048828125
          }
        },
        {
          "key_as_string": "2023-01-02T00:00:00.000Z",
          "key": 1672617600000,
          "doc_count": 1,
          "total_sales": {
            "value": 1269.06005859375
          }
        },
        {
          "key_as_string": "2023-03-15T00:00:00.000Z",
          "key": 1678838400000,
          "doc_count": 1,
          "total_sales": {
            "value": 1243.02001953125
          }
        },
        {
          "key_as_string": "2023-02-22T00:00:00.000Z",
          "key": 1677024000000,
          "doc_count": 1,
          "total_sales": {
            "value": 1149.5
          }
        },
        {
          "key_as_string": "2023-10-05T00:00:00.000Z",
          "key": 1696464000000,
          "doc_count": 1,
          "total_sales": {
            "value": 998.1599731445312
          }
        },
        {
          "key_as_string": "2023-12-18T00:00:00.000Z",
          "key": 1702857600000,
          "doc_count": 1,
          "total_sales": {
            "value": 978.1400146484375
          }
        },
        {
          "key_as_string": "2023-09-23T00:00:00.000Z",
          "key": 1695427200000,
          "doc_count": 1,
          "total_sales": {
            "value": 970.5999755859375
          }
        },
        {
          "key_as_string": "2023-12-01T00:00:00.000Z",
          "key": 1701388800000,
          "doc_count": 1,
          "total_sales": {
            "value": 960.1799926757812
          }
        },
        {
          "key_as_string": "2023-01-03T00:00:00.000Z",
          "key": 1672704000000,
          "doc_count": 1,
          "total_sales": {
            "value": 953.6699829101562
          }
        },
        {
          "key_as_string": "2023-06-07T00:00:00.000Z",
          "key": 1686096000000,
          "doc_count": 1,
          "total_sales": {
            "value": 904.010009765625
          }
        },
        {
          "key_as_string": "2023-06-23T00:00:00.000Z",
          "key": 1687478400000,
          "doc_count": 1,
          "total_sales": {
            "value": 861.1199951171875
          }
        },
        {
          "key_as_string": "2023-08-05T00:00:00.000Z",
          "key": 1691193600000,
          "doc_count": 1,
          "total_sales": {
            "value": 855
          }
        },
        {
          "key_as_string": "2023-02-17T00:00:00.000Z",
          "key": 1676592000000,
          "doc_count": 1,
          "total_sales": {
            "value": 847.1400146484375
          }
        },
        {
          "key_as_string": "2023-03-06T00:00:00.000Z",
          "key": 1678060800000,
          "doc_count": 1,
          "total_sales": {
            "value": 823.5599975585938
          }
        },
        {
          "key_as_string": "2023-05-10T00:00:00.000Z",
          "key": 1683676800000,
          "doc_count": 1,
          "total_sales": {
            "value": 811.2899780273438
          }
        },
        {
          "key_as_string": "2023-04-28T00:00:00.000Z",
          "key": 1682640000000,
          "doc_count": 1,
          "total_sales": {
            "value": 745.6500244140625
          }
        },
        {
          "key_as_string": "2023-08-02T00:00:00.000Z",
          "key": 1690934400000,
          "doc_count": 1,
          "total_sales": {
            "value": 732.4199829101562
          }
        },
        {
          "key_as_string": "2023-06-27T00:00:00.000Z",
          "key": 1687824000000,
          "doc_count": 1,
          "total_sales": {
            "value": 724.2000122070312
          }
        },
        {
          "key_as_string": "2023-06-12T00:00:00.000Z",
          "key": 1686528000000,
          "doc_count": 1,
          "total_sales": {
            "value": 698.1799926757812
          }
        },
        {
          "key_as_string": "2023-02-06T00:00:00.000Z",
          "key": 1675641600000,
          "doc_count": 1,
          "total_sales": {
            "value": 635.1199951171875
          }
        },
        {
          "key_as_string": "2023-03-17T00:00:00.000Z",
          "key": 1679011200000,
          "doc_count": 1,
          "total_sales": {
            "value": 602.760009765625
          }
        },
        {
          "key_as_string": "2023-03-21T00:00:00.000Z",
          "key": 1679356800000,
          "doc_count": 1,
          "total_sales": {
            "value": 600.6900024414062
          }
        },
        {
          "key_as_string": "2023-01-19T00:00:00.000Z",
          "key": 1674086400000,
          "doc_count": 1,
          "total_sales": {
            "value": 597.280029296875
          }
        },
        {
          "key_as_string": "2023-10-09T00:00:00.000Z",
          "key": 1696809600000,
          "doc_count": 1,
          "total_sales": {
            "value": 582.0399780273438
          }
        },
        {
          "key_as_string": "2023-12-19T00:00:00.000Z",
          "key": 1702944000000,
          "doc_count": 1,
          "total_sales": {
            "value": 575.8200073242188
          }
        },
        {
          "key_as_string": "2023-05-21T00:00:00.000Z",
          "key": 1684627200000,
          "doc_count": 2,
          "total_sales": {
            "value": 572.7400054931641
          }
        },
        {
          "key_as_string": "2023-03-12T00:00:00.000Z",
          "key": 1678579200000,
          "doc_count": 1,
          "total_sales": {
            "value": 535.02001953125
          }
        },
        {
          "key_as_string": "2023-03-03T00:00:00.000Z",
          "key": 1677801600000,
          "doc_count": 1,
          "total_sales": {
            "value": 489.239990234375
          }
        },
        {
          "key_as_string": "2023-10-10T00:00:00.000Z",
          "key": 1696896000000,
          "doc_count": 1,
          "total_sales": {
            "value": 447.79998779296875
          }
        },
        {
          "key_as_string": "2023-05-04T00:00:00.000Z",
          "key": 1683158400000,
          "doc_count": 1,
          "total_sales": {
            "value": 380.5199890136719
          }
        },
        {
          "key_as_string": "2023-05-29T00:00:00.000Z",
          "key": 1685318400000,
          "doc_count": 1,
          "total_sales": {
            "value": 379.510009765625
          }
        },
        {
          "key_as_string": "2023-07-13T00:00:00.000Z",
          "key": 1689206400000,
          "doc_count": 1,
          "total_sales": {
            "value": 363.17999267578125
          }
        },
        {
          "key_as_string": "2023-11-10T00:00:00.000Z",
          "key": 1699574400000,
          "doc_count": 1,
          "total_sales": {
            "value": 342.6300048828125
          }
        },
        {
          "key_as_string": "2023-02-18T00:00:00.000Z",
          "key": 1676678400000,
          "doc_count": 1,
          "total_sales": {
            "value": 307.6000061035156
          }
        },
        {
          "key_as_string": "2023-07-12T00:00:00.000Z",
          "key": 1689120000000,
          "doc_count": 1,
          "total_sales": {
            "value": 302.94000244140625
          }
        },
        {
          "key_as_string": "2023-04-16T00:00:00.000Z",
          "key": 1681603200000,
          "doc_count": 1,
          "total_sales": {
            "value": 291.9700012207031
          }
        },
        {
          "key_as_string": "2023-06-26T00:00:00.000Z",
          "key": 1687737600000,
          "doc_count": 1,
          "total_sales": {
            "value": 230.02000427246094
          }
        },
        {
          "key_as_string": "2023-06-25T00:00:00.000Z",
          "key": 1687651200000,
          "doc_count": 1,
          "total_sales": {
            "value": 212.85000610351562
          }
        },
        {
          "key_as_string": "2023-06-30T00:00:00.000Z",
          "key": 1688083200000,
          "doc_count": 1,
          "total_sales": {
            "value": 186.52000427246094
          }
        },
        {
          "key_as_string": "2023-08-25T00:00:00.000Z",
          "key": 1692921600000,
          "doc_count": 1,
          "total_sales": {
            "value": 29.280000686645508
          }
        },
        {
          "key_as_string": "2023-01-04T00:00:00.000Z",
          "key": 1672790400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-05T00:00:00.000Z",
          "key": 1672876800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-06T00:00:00.000Z",
          "key": 1672963200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-08T00:00:00.000Z",
          "key": 1673136000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-09T00:00:00.000Z",
          "key": 1673222400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-11T00:00:00.000Z",
          "key": 1673395200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-12T00:00:00.000Z",
          "key": 1673481600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-14T00:00:00.000Z",
          "key": 1673654400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-16T00:00:00.000Z",
          "key": 1673827200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-17T00:00:00.000Z",
          "key": 1673913600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-18T00:00:00.000Z",
          "key": 1674000000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-21T00:00:00.000Z",
          "key": 1674259200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-22T00:00:00.000Z",
          "key": 1674345600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-23T00:00:00.000Z",
          "key": 1674432000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-24T00:00:00.000Z",
          "key": 1674518400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-25T00:00:00.000Z",
          "key": 1674604800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-26T00:00:00.000Z",
          "key": 1674691200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-30T00:00:00.000Z",
          "key": 1675036800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-01-31T00:00:00.000Z",
          "key": 1675123200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-01T00:00:00.000Z",
          "key": 1675209600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-03T00:00:00.000Z",
          "key": 1675382400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-04T00:00:00.000Z",
          "key": 1675468800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-05T00:00:00.000Z",
          "key": 1675555200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-07T00:00:00.000Z",
          "key": 1675728000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-08T00:00:00.000Z",
          "key": 1675814400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-09T00:00:00.000Z",
          "key": 1675900800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-10T00:00:00.000Z",
          "key": 1675987200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-11T00:00:00.000Z",
          "key": 1676073600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-12T00:00:00.000Z",
          "key": 1676160000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-13T00:00:00.000Z",
          "key": 1676246400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-14T00:00:00.000Z",
          "key": 1676332800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-15T00:00:00.000Z",
          "key": 1676419200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-19T00:00:00.000Z",
          "key": 1676764800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-20T00:00:00.000Z",
          "key": 1676851200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-21T00:00:00.000Z",
          "key": 1676937600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-24T00:00:00.000Z",
          "key": 1677196800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-26T00:00:00.000Z",
          "key": 1677369600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-02-28T00:00:00.000Z",
          "key": 1677542400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-02T00:00:00.000Z",
          "key": 1677715200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-04T00:00:00.000Z",
          "key": 1677888000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-05T00:00:00.000Z",
          "key": 1677974400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-07T00:00:00.000Z",
          "key": 1678147200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-08T00:00:00.000Z",
          "key": 1678233600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-09T00:00:00.000Z",
          "key": 1678320000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-10T00:00:00.000Z",
          "key": 1678406400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-11T00:00:00.000Z",
          "key": 1678492800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-14T00:00:00.000Z",
          "key": 1678752000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-16T00:00:00.000Z",
          "key": 1678924800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-18T00:00:00.000Z",
          "key": 1679097600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-19T00:00:00.000Z",
          "key": 1679184000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-20T00:00:00.000Z",
          "key": 1679270400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-22T00:00:00.000Z",
          "key": 1679443200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-23T00:00:00.000Z",
          "key": 1679529600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-24T00:00:00.000Z",
          "key": 1679616000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-25T00:00:00.000Z",
          "key": 1679702400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-26T00:00:00.000Z",
          "key": 1679788800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-27T00:00:00.000Z",
          "key": 1679875200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-29T00:00:00.000Z",
          "key": 1680048000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-30T00:00:00.000Z",
          "key": 1680134400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-03-31T00:00:00.000Z",
          "key": 1680220800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-02T00:00:00.000Z",
          "key": 1680393600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-03T00:00:00.000Z",
          "key": 1680480000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-04T00:00:00.000Z",
          "key": 1680566400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-05T00:00:00.000Z",
          "key": 1680652800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-06T00:00:00.000Z",
          "key": 1680739200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-07T00:00:00.000Z",
          "key": 1680825600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-08T00:00:00.000Z",
          "key": 1680912000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-09T00:00:00.000Z",
          "key": 1680998400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-10T00:00:00.000Z",
          "key": 1681084800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-13T00:00:00.000Z",
          "key": 1681344000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-15T00:00:00.000Z",
          "key": 1681516800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-17T00:00:00.000Z",
          "key": 1681689600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-18T00:00:00.000Z",
          "key": 1681776000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-19T00:00:00.000Z",
          "key": 1681862400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-20T00:00:00.000Z",
          "key": 1681948800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-21T00:00:00.000Z",
          "key": 1682035200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-22T00:00:00.000Z",
          "key": 1682121600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-23T00:00:00.000Z",
          "key": 1682208000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-24T00:00:00.000Z",
          "key": 1682294400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-25T00:00:00.000Z",
          "key": 1682380800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-26T00:00:00.000Z",
          "key": 1682467200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-30T00:00:00.000Z",
          "key": 1682812800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-03T00:00:00.000Z",
          "key": 1683072000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-05T00:00:00.000Z",
          "key": 1683244800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-06T00:00:00.000Z",
          "key": 1683331200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-07T00:00:00.000Z",
          "key": 1683417600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-08T00:00:00.000Z",
          "key": 1683504000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-09T00:00:00.000Z",
          "key": 1683590400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-11T00:00:00.000Z",
          "key": 1683763200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-13T00:00:00.000Z",
          "key": 1683936000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-14T00:00:00.000Z",
          "key": 1684022400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-15T00:00:00.000Z",
          "key": 1684108800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-16T00:00:00.000Z",
          "key": 1684195200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-17T00:00:00.000Z",
          "key": 1684281600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-18T00:00:00.000Z",
          "key": 1684368000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-19T00:00:00.000Z",
          "key": 1684454400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-20T00:00:00.000Z",
          "key": 1684540800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-22T00:00:00.000Z",
          "key": 1684713600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-23T00:00:00.000Z",
          "key": 1684800000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-24T00:00:00.000Z",
          "key": 1684886400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-26T00:00:00.000Z",
          "key": 1685059200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-28T00:00:00.000Z",
          "key": 1685232000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-30T00:00:00.000Z",
          "key": 1685404800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-05-31T00:00:00.000Z",
          "key": 1685491200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-01T00:00:00.000Z",
          "key": 1685577600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-02T00:00:00.000Z",
          "key": 1685664000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-03T00:00:00.000Z",
          "key": 1685750400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-04T00:00:00.000Z",
          "key": 1685836800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-05T00:00:00.000Z",
          "key": 1685923200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-06T00:00:00.000Z",
          "key": 1686009600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-08T00:00:00.000Z",
          "key": 1686182400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-09T00:00:00.000Z",
          "key": 1686268800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-10T00:00:00.000Z",
          "key": 1686355200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-11T00:00:00.000Z",
          "key": 1686441600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-13T00:00:00.000Z",
          "key": 1686614400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-16T00:00:00.000Z",
          "key": 1686873600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-17T00:00:00.000Z",
          "key": 1686960000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-18T00:00:00.000Z",
          "key": 1687046400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-19T00:00:00.000Z",
          "key": 1687132800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-20T00:00:00.000Z",
          "key": 1687219200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-21T00:00:00.000Z",
          "key": 1687305600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-22T00:00:00.000Z",
          "key": 1687392000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-24T00:00:00.000Z",
          "key": 1687564800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-28T00:00:00.000Z",
          "key": 1687910400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-06-29T00:00:00.000Z",
          "key": 1687996800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-02T00:00:00.000Z",
          "key": 1688256000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-03T00:00:00.000Z",
          "key": 1688342400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-04T00:00:00.000Z",
          "key": 1688428800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-05T00:00:00.000Z",
          "key": 1688515200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-07T00:00:00.000Z",
          "key": 1688688000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-08T00:00:00.000Z",
          "key": 1688774400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-11T00:00:00.000Z",
          "key": 1689033600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-14T00:00:00.000Z",
          "key": 1689292800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-15T00:00:00.000Z",
          "key": 1689379200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-16T00:00:00.000Z",
          "key": 1689465600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-17T00:00:00.000Z",
          "key": 1689552000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-18T00:00:00.000Z",
          "key": 1689638400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-19T00:00:00.000Z",
          "key": 1689724800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-20T00:00:00.000Z",
          "key": 1689811200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-21T00:00:00.000Z",
          "key": 1689897600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-23T00:00:00.000Z",
          "key": 1690070400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-24T00:00:00.000Z",
          "key": 1690156800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-25T00:00:00.000Z",
          "key": 1690243200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-26T00:00:00.000Z",
          "key": 1690329600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-27T00:00:00.000Z",
          "key": 1690416000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-28T00:00:00.000Z",
          "key": 1690502400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-29T00:00:00.000Z",
          "key": 1690588800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-07-31T00:00:00.000Z",
          "key": 1690761600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-01T00:00:00.000Z",
          "key": 1690848000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-03T00:00:00.000Z",
          "key": 1691020800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-06T00:00:00.000Z",
          "key": 1691280000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-07T00:00:00.000Z",
          "key": 1691366400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-08T00:00:00.000Z",
          "key": 1691452800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-09T00:00:00.000Z",
          "key": 1691539200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-10T00:00:00.000Z",
          "key": 1691625600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-11T00:00:00.000Z",
          "key": 1691712000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-12T00:00:00.000Z",
          "key": 1691798400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-13T00:00:00.000Z",
          "key": 1691884800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-14T00:00:00.000Z",
          "key": 1691971200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-16T00:00:00.000Z",
          "key": 1692144000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-17T00:00:00.000Z",
          "key": 1692230400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-18T00:00:00.000Z",
          "key": 1692316800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-20T00:00:00.000Z",
          "key": 1692489600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-21T00:00:00.000Z",
          "key": 1692576000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-22T00:00:00.000Z",
          "key": 1692662400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-23T00:00:00.000Z",
          "key": 1692748800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-24T00:00:00.000Z",
          "key": 1692835200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-26T00:00:00.000Z",
          "key": 1693008000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-27T00:00:00.000Z",
          "key": 1693094400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-28T00:00:00.000Z",
          "key": 1693180800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-29T00:00:00.000Z",
          "key": 1693267200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-30T00:00:00.000Z",
          "key": 1693353600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-31T00:00:00.000Z",
          "key": 1693440000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-01T00:00:00.000Z",
          "key": 1693526400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-02T00:00:00.000Z",
          "key": 1693612800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-03T00:00:00.000Z",
          "key": 1693699200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-04T00:00:00.000Z",
          "key": 1693785600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-05T00:00:00.000Z",
          "key": 1693872000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-06T00:00:00.000Z",
          "key": 1693958400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-07T00:00:00.000Z",
          "key": 1694044800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-08T00:00:00.000Z",
          "key": 1694131200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-09T00:00:00.000Z",
          "key": 1694217600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-10T00:00:00.000Z",
          "key": 1694304000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-11T00:00:00.000Z",
          "key": 1694390400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-12T00:00:00.000Z",
          "key": 1694476800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-13T00:00:00.000Z",
          "key": 1694563200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-14T00:00:00.000Z",
          "key": 1694649600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-16T00:00:00.000Z",
          "key": 1694822400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-17T00:00:00.000Z",
          "key": 1694908800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-18T00:00:00.000Z",
          "key": 1694995200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-19T00:00:00.000Z",
          "key": 1695081600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-20T00:00:00.000Z",
          "key": 1695168000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-21T00:00:00.000Z",
          "key": 1695254400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-24T00:00:00.000Z",
          "key": 1695513600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-25T00:00:00.000Z",
          "key": 1695600000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-26T00:00:00.000Z",
          "key": 1695686400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-27T00:00:00.000Z",
          "key": 1695772800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-28T00:00:00.000Z",
          "key": 1695859200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-29T00:00:00.000Z",
          "key": 1695945600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-30T00:00:00.000Z",
          "key": 1696032000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-02T00:00:00.000Z",
          "key": 1696204800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-04T00:00:00.000Z",
          "key": 1696377600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-06T00:00:00.000Z",
          "key": 1696550400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-07T00:00:00.000Z",
          "key": 1696636800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-08T00:00:00.000Z",
          "key": 1696723200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-11T00:00:00.000Z",
          "key": 1696982400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-14T00:00:00.000Z",
          "key": 1697241600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-15T00:00:00.000Z",
          "key": 1697328000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-16T00:00:00.000Z",
          "key": 1697414400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-17T00:00:00.000Z",
          "key": 1697500800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-18T00:00:00.000Z",
          "key": 1697587200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-19T00:00:00.000Z",
          "key": 1697673600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-20T00:00:00.000Z",
          "key": 1697760000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-21T00:00:00.000Z",
          "key": 1697846400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-22T00:00:00.000Z",
          "key": 1697932800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-23T00:00:00.000Z",
          "key": 1698019200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-24T00:00:00.000Z",
          "key": 1698105600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-25T00:00:00.000Z",
          "key": 1698192000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-26T00:00:00.000Z",
          "key": 1698278400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-27T00:00:00.000Z",
          "key": 1698364800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-28T00:00:00.000Z",
          "key": 1698451200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-29T00:00:00.000Z",
          "key": 1698537600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-30T00:00:00.000Z",
          "key": 1698624000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-31T00:00:00.000Z",
          "key": 1698710400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-01T00:00:00.000Z",
          "key": 1698796800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-02T00:00:00.000Z",
          "key": 1698883200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-03T00:00:00.000Z",
          "key": 1698969600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-04T00:00:00.000Z",
          "key": 1699056000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-05T00:00:00.000Z",
          "key": 1699142400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-06T00:00:00.000Z",
          "key": 1699228800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-07T00:00:00.000Z",
          "key": 1699315200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-08T00:00:00.000Z",
          "key": 1699401600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-09T00:00:00.000Z",
          "key": 1699488000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-11T00:00:00.000Z",
          "key": 1699660800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-12T00:00:00.000Z",
          "key": 1699747200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-13T00:00:00.000Z",
          "key": 1699833600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-15T00:00:00.000Z",
          "key": 1700006400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-16T00:00:00.000Z",
          "key": 1700092800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-18T00:00:00.000Z",
          "key": 1700265600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-19T00:00:00.000Z",
          "key": 1700352000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-20T00:00:00.000Z",
          "key": 1700438400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-21T00:00:00.000Z",
          "key": 1700524800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-22T00:00:00.000Z",
          "key": 1700611200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-23T00:00:00.000Z",
          "key": 1700697600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-24T00:00:00.000Z",
          "key": 1700784000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-26T00:00:00.000Z",
          "key": 1700956800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-27T00:00:00.000Z",
          "key": 1701043200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-28T00:00:00.000Z",
          "key": 1701129600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-29T00:00:00.000Z",
          "key": 1701216000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-02T00:00:00.000Z",
          "key": 1701475200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-03T00:00:00.000Z",
          "key": 1701561600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-04T00:00:00.000Z",
          "key": 1701648000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-05T00:00:00.000Z",
          "key": 1701734400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-06T00:00:00.000Z",
          "key": 1701820800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-08T00:00:00.000Z",
          "key": 1701993600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-09T00:00:00.000Z",
          "key": 1702080000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-10T00:00:00.000Z",
          "key": 1702166400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-11T00:00:00.000Z",
          "key": 1702252800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-12T00:00:00.000Z",
          "key": 1702339200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-13T00:00:00.000Z",
          "key": 1702425600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-14T00:00:00.000Z",
          "key": 1702512000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-15T00:00:00.000Z",
          "key": 1702598400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-16T00:00:00.000Z",
          "key": 1702684800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-17T00:00:00.000Z",
          "key": 1702771200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-21T00:00:00.000Z",
          "key": 1703116800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-22T00:00:00.000Z",
          "key": 1703203200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-23T00:00:00.000Z",
          "key": 1703289600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-24T00:00:00.000Z",
          "key": 1703376000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-26T00:00:00.000Z",
          "key": 1703548800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-27T00:00:00.000Z",
          "key": 1703635200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-28T00:00:00.000Z",
          "key": 1703721600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-29T00:00:00.000Z",
          "key": 1703808000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-30T00:00:00.000Z",
          "key": 1703894400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/16.png)
### 数据分析--15.计算每个季度销售额最高的产品类别
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "quarterly_sales": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "top_category": {
          "terms": {
            "field": "product_category",
            "order": {
              "total_sales": "desc"
            }
          },
          "aggs": {
            "total_sales": {
              "sum": {
                "field": "total_amount"
              }
            }
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "quarterly_sales": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 33,
          "top_category": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "Jewelry",
                "doc_count": 8,
                "total_sales": {
                  "value": 11757.280212402344
                }
              },
              {
                "key": "Furniture",
                "doc_count": 4,
                "total_sales": {
                  "value": 8743.890014648438
                }
              },
              {
                "key": "Electronics",
                "doc_count": 5,
                "total_sales": {
                  "value": 8073.159973144531
                }
              },
              {
                "key": "Toys",
                "doc_count": 3,
                "total_sales": {
                  "value": 7987.280029296875
                }
              },
              {
                "key": "Home Appliances",
                "doc_count": 2,
                "total_sales": {
                  "value": 5443.77001953125
                }
              },
              {
                "key": "Beauty",
                "doc_count": 2,
                "total_sales": {
                  "value": 4612.440002441406
                }
              },
              {
                "key": "Books",
                "doc_count": 3,
                "total_sales": {
                  "value": 4522.739959716797
                }
              },
              {
                "key": "Groceries",
                "doc_count": 2,
                "total_sales": {
                  "value": 4123.97998046875
                }
              },
              {
                "key": "Sports",
                "doc_count": 2,
                "total_sales": {
                  "value": 3998.8600463867188
                }
              },
              {
                "key": "Fashion",
                "doc_count": 2,
                "total_sales": {
                  "value": 1948.3999633789062
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 28,
          "top_category": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "Home Appliances",
                "doc_count": 4,
                "total_sales": {
                  "value": 6474.219955444336
                }
              },
              {
                "key": "Electronics",
                "doc_count": 3,
                "total_sales": {
                  "value": 6231.419921875
                }
              },
              {
                "key": "Beauty",
                "doc_count": 3,
                "total_sales": {
                  "value": 5781.5098876953125
                }
              },
              {
                "key": "Groceries",
                "doc_count": 3,
                "total_sales": {
                  "value": 5680.110076904297
                }
              },
              {
                "key": "Furniture",
                "doc_count": 4,
                "total_sales": {
                  "value": 5408.469985961914
                }
              },
              {
                "key": "Fashion",
                "doc_count": 3,
                "total_sales": {
                  "value": 2786.490020751953
                }
              },
              {
                "key": "Jewelry",
                "doc_count": 3,
                "total_sales": {
                  "value": 1856.9000091552734
                }
              },
              {
                "key": "Toys",
                "doc_count": 3,
                "total_sales": {
                  "value": 1850.3700256347656
                }
              },
              {
                "key": "Books",
                "doc_count": 2,
                "total_sales": {
                  "value": 1190.7999877929688
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 18,
          "top_category": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "Home Appliances",
                "doc_count": 3,
                "total_sales": {
                  "value": 6066.1199951171875
                }
              },
              {
                "key": "Sports",
                "doc_count": 1,
                "total_sales": {
                  "value": 4886.10009765625
                }
              },
              {
                "key": "Books",
                "doc_count": 2,
                "total_sales": {
                  "value": 4864.370178222656
                }
              },
              {
                "key": "Electronics",
                "doc_count": 3,
                "total_sales": {
                  "value": 3483.2700805664062
                }
              },
              {
                "key": "Furniture",
                "doc_count": 2,
                "total_sales": {
                  "value": 3075.9599609375
                }
              },
              {
                "key": "Beauty",
                "doc_count": 2,
                "total_sales": {
                  "value": 2898.10009765625
                }
              },
              {
                "key": "Groceries",
                "doc_count": 1,
                "total_sales": {
                  "value": 2173.02001953125
                }
              },
              {
                "key": "Fashion",
                "doc_count": 2,
                "total_sales": {
                  "value": 1360.0800495147705
                }
              },
              {
                "key": "Toys",
                "doc_count": 1,
                "total_sales": {
                  "value": 691.1799926757812
                }
              },
              {
                "key": "Jewelry",
                "doc_count": 1,
                "total_sales": {
                  "value": 363.17999267578125
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 21,
          "top_category": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "Beauty",
                "doc_count": 4,
                "total_sales": {
                  "value": 9416.89013671875
                }
              },
              {
                "key": "Home Appliances",
                "doc_count": 4,
                "total_sales": {
                  "value": 7881.080261230469
                }
              },
              {
                "key": "Sports",
                "doc_count": 3,
                "total_sales": {
                  "value": 4365.539978027344
                }
              },
              {
                "key": "Groceries",
                "doc_count": 3,
                "total_sales": {
                  "value": 4195.8699951171875
                }
              },
              {
                "key": "Jewelry",
                "doc_count": 2,
                "total_sales": {
                  "value": 3500.0100708007812
                }
              },
              {
                "key": "Books",
                "doc_count": 2,
                "total_sales": {
                  "value": 1300.9099731445312
                }
              },
              {
                "key": "Electronics",
                "doc_count": 2,
                "total_sales": {
                  "value": 1153.7100219726562
                }
              },
              {
                "key": "Fashion",
                "doc_count": 1,
                "total_sales": {
                  "value": 978.1400146484375
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/17.png)
### 数据分析--16.计算每天的订单数量，并显示7天移动平均值
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "daily_orders": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day"
      },
      "aggs": {
        "order_count": {
          "value_count": {
            "field": "order_id"
          }
        },
        "moving_avg": {
          "moving_avg": {
            "buckets_path": "order_count",
            "window": 7,
            "model": "simple"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "error": {
    "root_cause": [
      {
        "type": "parsing_exception",
        "reason": "Unknown aggregation type [moving_avg] did you mean [moving_fn]?",
        "line": 16,
        "col": 25
      }
    ],
    "type": "parsing_exception",
    "reason": "Unknown aggregation type [moving_avg] did you mean [moving_fn]?",
    "line": 16,
    "col": 25,
    "caused_by": {
      "type": "named_object_not_found_exception",
      "reason": "[16:25] unknown field [moving_avg]"
    }
  },
  "status": 400
}
```
### 示例图片--
![Markdown Logo](./Images/18.png)
### 数据分析--17.比较本月销售额与上月销售额的差异
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "monthly_sales": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "month"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "monthly_sales": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 12,
          "total_sales": {
            "value": 24381.61993408203
          }
        },
        {
          "key_as_string": "2023-02-01T00:00:00.000Z",
          "key": 1675209600000,
          "doc_count": 11,
          "total_sales": {
            "value": 18870.850006103516
          }
        },
        {
          "key_as_string": "2023-03-01T00:00:00.000Z",
          "key": 1677628800000,
          "doc_count": 10,
          "total_sales": {
            "value": 17959.33026123047
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 9,
          "total_sales": {
            "value": 18775.959869384766
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 10,
          "total_sales": {
            "value": 11713.169967651367
          }
        },
        {
          "key_as_string": "2023-06-01T00:00:00.000Z",
          "key": 1685577600000,
          "doc_count": 9,
          "total_sales": {
            "value": 6771.1600341796875
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 7,
          "total_sales": {
            "value": 9110.010131835938
          }
        },
        {
          "key_as_string": "2023-08-01T00:00:00.000Z",
          "key": 1690848000000,
          "doc_count": 8,
          "total_sales": {
            "value": 12135.870210647583
          }
        },
        {
          "key_as_string": "2023-09-01T00:00:00.000Z",
          "key": 1693526400000,
          "doc_count": 3,
          "total_sales": {
            "value": 8615.500122070312
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 6,
          "total_sales": {
            "value": 12106.000183105469
          }
        },
        {
          "key_as_string": "2023-11-01T00:00:00.000Z",
          "key": 1698796800000,
          "doc_count": 6,
          "total_sales": {
            "value": 8176.529968261719
          }
        },
        {
          "key_as_string": "2023-12-01T00:00:00.000Z",
          "key": 1701388800000,
          "doc_count": 9,
          "total_sales": {
            "value": 12509.620300292969
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/19.png)
### 数据分析--18.计算每周的总销售额，并找出销售额增长最快的一周
```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "weekly_sales": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "week"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        },
        "sales_diff": {
          "serial_diff": {
            "buckets_path": "total_sales"
          }
        }
      }
    }
  }
}

```

### 结果--
```
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "weekly_sales": {
      "buckets": [
        {
          "key_as_string": "2023-01-02T00:00:00.000Z",
          "key": 1672617600000,
          "doc_count": 3,
          "total_sales": {
            "value": 4912.139953613281
          }
        },
        {
          "key_as_string": "2023-01-09T00:00:00.000Z",
          "key": 1673222400000,
          "doc_count": 3,
          "total_sales": {
            "value": 8571.68994140625
          },
          "sales_diff": {
            "value": 3659.5499877929688
          }
        },
        {
          "key_as_string": "2023-01-16T00:00:00.000Z",
          "key": 1673827200000,
          "doc_count": 3,
          "total_sales": {
            "value": 3978.7000732421875
          },
          "sales_diff": {
            "value": -4592.9898681640625
          }
        },
        {
          "key_as_string": "2023-01-23T00:00:00.000Z",
          "key": 1674432000000,
          "doc_count": 3,
          "total_sales": {
            "value": 6919.0899658203125
          },
          "sales_diff": {
            "value": 2940.389892578125
          }
        },
        {
          "key_as_string": "2023-01-30T00:00:00.000Z",
          "key": 1675036800000,
          "doc_count": 1,
          "total_sales": {
            "value": 3605.800048828125
          },
          "sales_diff": {
            "value": -3313.2899169921875
          }
        },
        {
          "key_as_string": "2023-02-06T00:00:00.000Z",
          "key": 1675641600000,
          "doc_count": 1,
          "total_sales": {
            "value": 635.1199951171875
          },
          "sales_diff": {
            "value": -2970.6800537109375
          }
        },
        {
          "key_as_string": "2023-02-13T00:00:00.000Z",
          "key": 1676246400000,
          "doc_count": 3,
          "total_sales": {
            "value": 3622.939971923828
          },
          "sales_diff": {
            "value": 2987.8199768066406
          }
        },
        {
          "key_as_string": "2023-02-20T00:00:00.000Z",
          "key": 1676851200000,
          "doc_count": 4,
          "total_sales": {
            "value": 7788.419982910156
          },
          "sales_diff": {
            "value": 4165.480010986328
          }
        },
        {
          "key_as_string": "2023-02-27T00:00:00.000Z",
          "key": 1677456000000,
          "doc_count": 5,
          "total_sales": {
            "value": 9165.450073242188
          },
          "sales_diff": {
            "value": 1377.0300903320312
          }
        },
        {
          "key_as_string": "2023-03-06T00:00:00.000Z",
          "key": 1678060800000,
          "doc_count": 2,
          "total_sales": {
            "value": 1358.5800170898438
          },
          "sales_diff": {
            "value": -7806.870056152344
          }
        },
        {
          "key_as_string": "2023-03-13T00:00:00.000Z",
          "key": 1678665600000,
          "doc_count": 3,
          "total_sales": {
            "value": 6438.730224609375
          },
          "sales_diff": {
            "value": 5080.150207519531
          }
        },
        {
          "key_as_string": "2023-03-20T00:00:00.000Z",
          "key": 1679270400000,
          "doc_count": 1,
          "total_sales": {
            "value": 600.6900024414062
          },
          "sales_diff": {
            "value": -5838.040222167969
          }
        },
        {
          "key_as_string": "2023-03-27T00:00:00.000Z",
          "key": 1679875200000,
          "doc_count": 2,
          "total_sales": {
            "value": 4943.72998046875
          },
          "sales_diff": {
            "value": 4343.039978027344
          }
        },
        {
          "key_as_string": "2023-04-03T00:00:00.000Z",
          "key": 1680480000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-10T00:00:00.000Z",
          "key": 1681084800000,
          "doc_count": 4,
          "total_sales": {
            "value": 8707.539825439453
          }
        },
        {
          "key_as_string": "2023-04-17T00:00:00.000Z",
          "key": 1681689600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-04-24T00:00:00.000Z",
          "key": 1682294400000,
          "doc_count": 4,
          "total_sales": {
            "value": 8739.140014648438
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 2,
          "total_sales": {
            "value": 3348.029998779297
          },
          "sales_diff": {
            "value": -5391.110015869141
          }
        },
        {
          "key_as_string": "2023-05-08T00:00:00.000Z",
          "key": 1683504000000,
          "doc_count": 3,
          "total_sales": {
            "value": 2923.4099731445312
          },
          "sales_diff": {
            "value": -424.6200256347656
          }
        },
        {
          "key_as_string": "2023-05-15T00:00:00.000Z",
          "key": 1684108800000,
          "doc_count": 2,
          "total_sales": {
            "value": 572.7400054931641
          },
          "sales_diff": {
            "value": -2350.669967651367
          }
        },
        {
          "key_as_string": "2023-05-22T00:00:00.000Z",
          "key": 1684713600000,
          "doc_count": 2,
          "total_sales": {
            "value": 4489.47998046875
          },
          "sales_diff": {
            "value": 3916.739974975586
          }
        },
        {
          "key_as_string": "2023-05-29T00:00:00.000Z",
          "key": 1685318400000,
          "doc_count": 1,
          "total_sales": {
            "value": 379.510009765625
          },
          "sales_diff": {
            "value": -4109.969970703125
          }
        },
        {
          "key_as_string": "2023-06-05T00:00:00.000Z",
          "key": 1685923200000,
          "doc_count": 1,
          "total_sales": {
            "value": 904.010009765625
          },
          "sales_diff": {
            "value": 524.5
          }
        },
        {
          "key_as_string": "2023-06-12T00:00:00.000Z",
          "key": 1686528000000,
          "doc_count": 3,
          "total_sales": {
            "value": 3652.4400024414062
          },
          "sales_diff": {
            "value": 2748.4299926757812
          }
        },
        {
          "key_as_string": "2023-06-19T00:00:00.000Z",
          "key": 1687132800000,
          "doc_count": 2,
          "total_sales": {
            "value": 1073.9700012207031
          },
          "sales_diff": {
            "value": -2578.470001220703
          }
        },
        {
          "key_as_string": "2023-06-26T00:00:00.000Z",
          "key": 1687737600000,
          "doc_count": 3,
          "total_sales": {
            "value": 1140.7400207519531
          },
          "sales_diff": {
            "value": 66.77001953125
          }
        },
        {
          "key_as_string": "2023-07-03T00:00:00.000Z",
          "key": 1688342400000,
          "doc_count": 2,
          "total_sales": {
            "value": 3268.2000732421875
          },
          "sales_diff": {
            "value": 2127.4600524902344
          }
        },
        {
          "key_as_string": "2023-07-10T00:00:00.000Z",
          "key": 1688947200000,
          "doc_count": 3,
          "total_sales": {
            "value": 1996.9200439453125
          },
          "sales_diff": {
            "value": -1271.280029296875
          }
        },
        {
          "key_as_string": "2023-07-17T00:00:00.000Z",
          "key": 1689552000000,
          "doc_count": 1,
          "total_sales": {
            "value": 2220.9599609375
          },
          "sales_diff": {
            "value": 224.0399169921875
          }
        },
        {
          "key_as_string": "2023-07-24T00:00:00.000Z",
          "key": 1690156800000,
          "doc_count": 1,
          "total_sales": {
            "value": 1623.9300537109375
          },
          "sales_diff": {
            "value": -597.0299072265625
          }
        },
        {
          "key_as_string": "2023-07-31T00:00:00.000Z",
          "key": 1690761600000,
          "doc_count": 3,
          "total_sales": {
            "value": 5719.370178222656
          },
          "sales_diff": {
            "value": 4095.4401245117188
          }
        },
        {
          "key_as_string": "2023-08-07T00:00:00.000Z",
          "key": 1691366400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-08-14T00:00:00.000Z",
          "key": 1691971200000,
          "doc_count": 4,
          "total_sales": {
            "value": 6387.220031738281
          }
        },
        {
          "key_as_string": "2023-08-21T00:00:00.000Z",
          "key": 1692576000000,
          "doc_count": 1,
          "total_sales": {
            "value": 29.280000686645508
          },
          "sales_diff": {
            "value": -6357.940031051636
          }
        },
        {
          "key_as_string": "2023-08-28T00:00:00.000Z",
          "key": 1693180800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-04T00:00:00.000Z",
          "key": 1693785600000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-09-11T00:00:00.000Z",
          "key": 1694390400000,
          "doc_count": 1,
          "total_sales": {
            "value": 4886.10009765625
          }
        },
        {
          "key_as_string": "2023-09-18T00:00:00.000Z",
          "key": 1694995200000,
          "doc_count": 2,
          "total_sales": {
            "value": 3729.4000244140625
          },
          "sales_diff": {
            "value": -1156.7000732421875
          }
        },
        {
          "key_as_string": "2023-09-25T00:00:00.000Z",
          "key": 1695600000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-02T00:00:00.000Z",
          "key": 1696204800000,
          "doc_count": 2,
          "total_sales": {
            "value": 5857.110168457031
          }
        },
        {
          "key_as_string": "2023-10-09T00:00:00.000Z",
          "key": 1696809600000,
          "doc_count": 4,
          "total_sales": {
            "value": 6248.8900146484375
          },
          "sales_diff": {
            "value": 391.77984619140625
          }
        },
        {
          "key_as_string": "2023-10-16T00:00:00.000Z",
          "key": 1697414400000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-23T00:00:00.000Z",
          "key": 1698019200000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-10-30T00:00:00.000Z",
          "key": 1698624000000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-11-06T00:00:00.000Z",
          "key": 1699228800000,
          "doc_count": 1,
          "total_sales": {
            "value": 342.6300048828125
          }
        },
        {
          "key_as_string": "2023-11-13T00:00:00.000Z",
          "key": 1699833600000,
          "doc_count": 2,
          "total_sales": {
            "value": 3917.739990234375
          },
          "sales_diff": {
            "value": 3575.1099853515625
          }
        },
        {
          "key_as_string": "2023-11-20T00:00:00.000Z",
          "key": 1700438400000,
          "doc_count": 2,
          "total_sales": {
            "value": 1300.9099731445312
          },
          "sales_diff": {
            "value": -2616.8300170898438
          }
        },
        {
          "key_as_string": "2023-11-27T00:00:00.000Z",
          "key": 1701043200000,
          "doc_count": 2,
          "total_sales": {
            "value": 3575.4299926757812
          },
          "sales_diff": {
            "value": 2274.52001953125
          }
        },
        {
          "key_as_string": "2023-12-04T00:00:00.000Z",
          "key": 1701648000000,
          "doc_count": 1,
          "total_sales": {
            "value": 1498.1099853515625
          },
          "sales_diff": {
            "value": -2077.3200073242188
          }
        },
        {
          "key_as_string": "2023-12-11T00:00:00.000Z",
          "key": 1702252800000,
          "doc_count": 0,
          "total_sales": {
            "value": 0
          }
        },
        {
          "key_as_string": "2023-12-18T00:00:00.000Z",
          "key": 1702857600000,
          "doc_count": 3,
          "total_sales": {
            "value": 2833.2600708007812
          }
        },
        {
          "key_as_string": "2023-12-25T00:00:00.000Z",
          "key": 1703462400000,
          "doc_count": 4,
          "total_sales": {
            "value": 7218.070251464844
          },
          "sales_diff": {
            "value": 4384.8101806640625
          }
        }
      ]
    }
  }
}
```
### 示例图片--
![Markdown Logo](./Images/20.png)

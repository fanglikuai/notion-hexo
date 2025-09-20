---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YAYPT2ZV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQDhy3AVM%2BoxdNfJNGqCZc1M2ogqiABo0xEFlIXN2Z4hXQIgPxmlmA9l0uND2sGw2JxWFx7TSZcWI9P24xjoPvaeJskqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPbLDmnAIEfgJZ3%2FQSrcA1eU5CFIIkus44CwpvlcEicSlii5UjPiwwxGbW9BvyT1msuk4EUfrPOf%2FW9tEiI87UT%2FQ8j8oGAa2RU6zrsJCSfs9GwtllgmYiMwqBfNtYtM7EpAFy1WVAtb0fjQoNzgNNttx0j8MRS7zC8hCTqnle9U%2FWDX6ti6u9STRw1GZn6Z9ndOeAW3gG1xrlMn1JP85ErIwdC38lH4NrbuJZ7xSx59PTGbtbip4VQcl%2FXoF%2B565Bc3%2Bqf68Rb9y8MPcByyc6zIMBxtmxQOiTUXRsZwum9qdn%2FGhX1VAootr%2FzxU%2FNfxEoTqpQKlEi4xuxtlonUaOLY12BkdZT8cBMykBxA3LiYA%2BJE1NIqV0QhBew0dYWgqy92SbzeqPqzF6g71KqTTkPBbEnbb%2BjWmiVy%2FIsnJdlYK5WjXF6ZVGqo13crmrXsT0nvtiUT1zmZ9iHYH2Gz20x8c%2BJttXABEJh%2BVyuYSVckcwrEj8zl7TDskSKHJhhlObkP5%2BVc77jE65dLfxEft4rQfm%2BwsmnE7Mum%2B6FZy%2Bp%2FtRyreuRWVy3%2Bb18ACWfmFoFnZGXpCx8SxYnB9Y63K4OBSeNNCDC8kIxllwS8HLBeVH6M36zXGvjcmbuG4mmhU%2FINTp2px6iqREcTMK%2FqucYGOqUBCCB%2BqfuCywSGIHu22sH4FEf2JPoEAf0oRyNReigASJh5jyYYoFB2q0SERuyJNnIk4T9zKGjj3ycT46E3mY6Q6ehiKJjl1cTxycebFxYHshioqneozlC3cHNSvr2%2Bnh6q6YOeDreHSpQ2zuXAleA%2F1M%2B6lsjgWcHomJHsS6FntUj848n14FK0RgB0g30zUwDLwqSZMPKdTxUXwg6F9nM6VsW15x7U&X-Amz-Signature=17f0de4a3c42df094d76aba1532b380bd7c40de27370e4703939c4933e50c2d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


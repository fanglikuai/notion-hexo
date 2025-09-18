---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WYFWDE7%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDYkXq8zpBvPCnXoHImTc%2FbPcZWLg3qdPW8BMyH%2BwQXMgIhAKjZd5fZXb1ohOHIUTBAjNsTPK2RBn2mXdFETyWsE2M9KogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyQgiwQt4xE4ZG%2B1jIq3AOJ6FtyuKzEUqaPyHMLNDofGdERUiqKrshfTWrIOLG%2FD6b1H48uwRMu2WKL1zkOY3NyMto7zTnPCIDODb65ZEbcexILioqc%2Bt3Xke35WCazNmF7W%2FiBV2%2FHCDdd%2Fx9KPcRJ6yMUr%2FN2I5tU6au%2FcERN%2FDEJvuKmXN1UYa7t4zUxjQBP60XjndRxsfztqHA5O4TwDB9uMg9RqlJl5Vu2GKoxhwSMAoHwmf3AQWwTYKKnQThJ8RZ4KQd20XGpu08OtY%2FP3l5hOkCdhMOkoIfi82Jc6%2FIdfkg94JAeZsxfUoJukYooF8XYZlKelPWvEupBGHawbeBFEfL%2B%2BWn4ORFyRpQN9i0G3seR8tGlXJ6U2C5nrtHnZOxf08oDxcCBOiAnyT78%2FpvWorcJvhV%2BaBpPm8QUa24Nyyq9ghXHNMUn906iVJgKOQjWjkfBQnPgCVyp6980FPHt5IDwkqRHp04m8rxhRUCZb2CxppZ%2BTjzjvLh5Oozrzb9o2J0EqqY%2FL8CTQN0g266v35lkcsYom7aqfQMLyPKC5G5q2tWNANDiqvL7XA89nHFYAjnZv4pMg%2B3VhwUtHNC5%2BNTkbhW88saVeVqnF1AmzZ2SNCV9z7Vus9marU%2B0HNOqFtAzMMfVnDD4ma3GBjqkATx1duuoGEkjSWu%2F9AAghAtTUxF79xqgR7OlXLtx%2F4Z8Y2iTeYqQch0TIQaQakgYNXm6%2F86rFTOWrjpChOvnHGz49ZeynruI5nATeOsdRm5GY2xZId6S0p0yLoszs%2BUDWe5fMefZ6AruuiYGML0u3SQVdCMi9sOKsXx%2F1xBee3ts%2FhhqHLR0JF9ApwZ%2FC%2FZR316Asf%2FATVg5e%2F6mgPVXJqffTSFu&X-Amz-Signature=506c4bb797d0f183c8c85c722f40591de838f80e6b76f764ec6a700bae30d89a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


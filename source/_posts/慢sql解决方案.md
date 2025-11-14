---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667JEW2566%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCG%2FKagh2jm%2F%2FookKHaXPTyMDWodSj9tP%2FYbQ%2BDnKvYJAIhAORH%2FlSGN6KRw6GHWuoafpRdCCe%2BrqqYJ72Ap0Pqcr6gKv8DCGEQABoMNjM3NDIzMTgzODA1Igwv5hpx91cshR1cZZoq3APo7pnBXZe3HQzpbUFs8y4mIso9ISqgq44vz%2F2UuXL1jD1tvtCYTVXVdVbT83gB%2FR5Aufc0wKQGM42rizov5iRWfvlh2uYZPrqDgQfVgZtppsQF9qDBDw44ZQDKxX9uCDmqwB2lPRSJJo4nnFTaXypFo0GLG0HTV8Ih3cGPaTPAh1pp3Evp%2FPManqlWh4Zc8d9A0jYxDKIXDUFcGs7LsnIaaz0L6g9c0zXU1n39zFYUVvztJ8oDIcZsaipZB2O32VI64Di8ArTtsmBb1qgmZFUEhYoyhOxo%2FCxoUKnOJNRs1NYE6BNGwygt1Br%2BKAq4CGRJpKTEwyCwbI%2B19d2Xuq%2FLMiYcUL6dN0v1k0hrt3Air%2FqFv4OPJ2XSe7VpQLhuUUkvlnzI3AYPflkmzPaiyXyG8sKigMFL6OkSCo8hFlxdAFQxu326aMHnd0MraIkHWkCwOS2RMx7ayONkuK3UvdpZliDXts%2BHbRws6tBkLG2zTe2TC9DEIeDyMi8%2BTLV%2FzuTAfH1iZwHFWai%2BVoNyvGL%2BIdBWQJQjAfFpqj%2B1K6bUrylayWmDQ7Es730aU5nIGdGKP10woG5gGGOdrU6eRU3z8P9VDJ0Uo3XphjtNV09kOhRByul8FqjxyQcfTDCIwdvIBjqkAc2uoER%2FEJd49sDb7zJiIFWktxYfgLMJXf1YwcuDTWeFGUFAJN2CiPNrEwKYLsRxkzqPqTQySLJ8cPCSNf1fWSmNAneyTAI%2BIbTrIIaPg4J%2FLuQyiO3vzE6sfsQ9CRl24m8s9K54K8QZT4SJ%2FTkzWiaOd6BIc%2BiwISx9r8wuODuuH0BdwnksHg%2BXQ2iK6QpByYNOVT%2FS9chAQIB%2F7Xx0ptsq1dt7&X-Amz-Signature=a0a4a3a253c659ebfbcb3a82d33fb2e76f197354150d6d4e970701315c87d1d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OP7IUXR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDJq1vHvIduuIYhvZrkEeUm3FKYyueM4ZVFVqd8mmnlNQIhANFXjgytbinGKHM%2F1nRmbZTM9KWlS8dH0FKkuEJnA4HhKv8DCCQQABoMNjM3NDIzMTgzODA1Igytdhag5JXXWzgJsoYq3AOiRyDK6qJooE8kdqEBSfnC0JtPA0MoT%2FsLBoERjaXrldaH70oU61Ckq2hnSKINGORvaIiNt6kV5gh1LAlN2Q7%2FFa4WSR1ddnD0MtoChPAe9zljPkFORi9kvxPd7LpgZsrW4QvhrLPc6czGaBbPl8gIthBgnUNItr%2BQUN2mK22yYrLDUPjQbv1X3GVO37inxZ8OJli%2BKmyAs4gEvwAWsqo0OcrYdslIC1S3573UZkepjzA8F9EcuJVMII%2FsxX%2FA%2BfyhRHUVIIrlxNhhSI%2FJ%2Fx8F%2BU%2BMMaUWiUNhk6k4AtVbnBzYwyxo9vCaJkJcZ6o3bZVRxVo5prPGdQtTZPMvJKIe7u9P53K2uFWgcFZ8yn4KnH1UpKMnmmpYD1GrR9cKRkXkkChq9tVRtu8O0wa9zvECHQbnXSaO%2Fe8nusQ%2BfFxnEMWrEnoZEknwcInjL%2BTwb5rByt9NtNwD8a1dc0BxVFntf2ghq7jDKDazU3JS%2BFm%2B2bxR4dYHRkBQSozvHLl1GWvCM4zt9w5W5Cu1H0wQKlBIFdyRutwac%2BcAdRAvb8S3dQe1ysQLGmsPkkg3S%2B1OfVkqyfPKwvHX5Me6to1aflB%2BOvrcZh7ORJ09eUgdiPdxSFty%2B0J8q7w4CYWMSjD6%2F83IBjqkActI1%2BI0lNzfrA8hv6Kbb86UCwX%2FQngL34uMqr6rNmQzE8wYLTwWNtbenjWYWnPGNd8DxeuDYew3oZBFcebNYTGg8ozgUoB3adR794G7KtIQub5ZPDGRTbj5k5FWJRsHxTZ9OyVsVmoxg8ze%2FvIsKTva69wa9soid3jii0IYEKazVV0eY5BtplvLEqs4xCMI5B2YqKMGBXltsAOrlW9RHE5lRzkW&X-Amz-Signature=e3add4c1d7ff3a855a489aa4e19bd9042a8af9e06bbe3d9518687a0f92727893&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


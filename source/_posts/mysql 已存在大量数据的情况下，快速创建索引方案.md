---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VMWVIAT%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T000037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJIMEYCIQDMmSTrLTPg7XR6NG3NEIPWTbX5YM7RXS7zMMp9%2BVy1XgIhANw3eUTV9%2BGHVt%2FzqiC0fjCvErXYlWJ4h7c1oC0fhEicKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxkFcudclag1FBfSw4q3AMOT6LUrEl%2BbuMOrPhQ0x0jNmAher0cgdwedY9v78Ly3a3wseSeQFHt1Y%2F20yGNPG19jC32xe2143%2B%2F7UptqyO0sXqBkcSRkKy6Wh%2FHLfedEvByptt5HJeooxpMcydvpUQB8C%2BVfLkRh%2F1DGqhvgP4mV%2FJc9yuB20dngm5iF7FwK7jb%2B9%2B30OIitBm4L0XJJEfTtl2Yrd2smfTwPiziM7t2qT5cWruwzT8Pfe0uoC3z1CEwhkZyFcih8JPwdSejDPQc%2B5aWAjKRXSvD2rk8Z8qeczZCOAxfkFOheUw5eXonp%2FCrY1O1VqaQo5L%2BnNC7wHS29KRBx6tK3j3Z02RKi%2FFK3LvYsNF5hypRG1QhMZE4KKj3iVlV8GT%2Fyw3j0es67DeB1Tknafr9A1lcsUOfQEMIpbdc4%2BHlNGQoaCU%2FsFO94JMdESn98xAvvqk2pGStCp0BI568T4vuvHi%2FnSs9rkfQbxEPXDIDDlI8B8Z0Csg76RRWNq55qhv3HMMW2qfyzh1o2QB0rCCuPwuf7%2BGvBpOBfuuIY1PMCPe%2BVHwikzmP3mdavYgVJTruZr8zDg5Ye5cDds5DkGI9ZT5UA0F8y6t1AUzj%2F7AfSPSVmf5FVOP56BCnFjXYxCMibiO0CzDp%2FbnIBjqkAa5g4P654ratxWS5cgWy75pfNxhtnDW8ZUc0IFz8GyxW6k1O8FxLQggig9TxZ%2F9ljpt7GcnWTnmtoQbz4WcUshXfkLXieH1rtfqk42J0piTMZZwZteHJH7YGhoB%2Fbz6hrnq0l6sMuoZYnFAq34RZT%2Fgg3QEW2mzxQGfQ63ink72EdICx%2B8qVyT1YdXFJUNNG5Tsta4YTjIt%2FgAQh3BXhwdVje%2BxX&X-Amz-Signature=2a3b06f1b7199f4cf26a5ea979f2178d7a8fe637d7d903c5f4829a346e188f43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


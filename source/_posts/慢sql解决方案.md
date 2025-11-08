---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VMWVIAT%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T000037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJIMEYCIQDMmSTrLTPg7XR6NG3NEIPWTbX5YM7RXS7zMMp9%2BVy1XgIhANw3eUTV9%2BGHVt%2FzqiC0fjCvErXYlWJ4h7c1oC0fhEicKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxkFcudclag1FBfSw4q3AMOT6LUrEl%2BbuMOrPhQ0x0jNmAher0cgdwedY9v78Ly3a3wseSeQFHt1Y%2F20yGNPG19jC32xe2143%2B%2F7UptqyO0sXqBkcSRkKy6Wh%2FHLfedEvByptt5HJeooxpMcydvpUQB8C%2BVfLkRh%2F1DGqhvgP4mV%2FJc9yuB20dngm5iF7FwK7jb%2B9%2B30OIitBm4L0XJJEfTtl2Yrd2smfTwPiziM7t2qT5cWruwzT8Pfe0uoC3z1CEwhkZyFcih8JPwdSejDPQc%2B5aWAjKRXSvD2rk8Z8qeczZCOAxfkFOheUw5eXonp%2FCrY1O1VqaQo5L%2BnNC7wHS29KRBx6tK3j3Z02RKi%2FFK3LvYsNF5hypRG1QhMZE4KKj3iVlV8GT%2Fyw3j0es67DeB1Tknafr9A1lcsUOfQEMIpbdc4%2BHlNGQoaCU%2FsFO94JMdESn98xAvvqk2pGStCp0BI568T4vuvHi%2FnSs9rkfQbxEPXDIDDlI8B8Z0Csg76RRWNq55qhv3HMMW2qfyzh1o2QB0rCCuPwuf7%2BGvBpOBfuuIY1PMCPe%2BVHwikzmP3mdavYgVJTruZr8zDg5Ye5cDds5DkGI9ZT5UA0F8y6t1AUzj%2F7AfSPSVmf5FVOP56BCnFjXYxCMibiO0CzDp%2FbnIBjqkAa5g4P654ratxWS5cgWy75pfNxhtnDW8ZUc0IFz8GyxW6k1O8FxLQggig9TxZ%2F9ljpt7GcnWTnmtoQbz4WcUshXfkLXieH1rtfqk42J0piTMZZwZteHJH7YGhoB%2Fbz6hrnq0l6sMuoZYnFAq34RZT%2Fgg3QEW2mzxQGfQ63ink72EdICx%2B8qVyT1YdXFJUNNG5Tsta4YTjIt%2FgAQh3BXhwdVje%2BxX&X-Amz-Signature=8a52168e0e1a1d677ea1e058eb27ab4dad3eff13f0646d8aaf15c5bfa6f457fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


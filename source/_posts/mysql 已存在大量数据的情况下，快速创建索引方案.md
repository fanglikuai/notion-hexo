---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TZXVFUW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSNS9w99tJV6pVZh%2FDEEtSAq30bfPFNqHeN15jG1oS8gIhALAZJ3d8PyP8H%2B1H%2BkIqt%2BNhjk5CFbZHHAdQTCqV%2FWs0KogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzLbYBIjPDJuqhWJmkq3AOBYewajdE0yGhChKieUw2oW9biSkkEVrCnj1Tn61AH90uafBTHdGxnY1hSQHmCHxwrmfWnzYUlyJ1oda8bMm7DpeWqidTnLH26%2B3%2BVVyPeApWZ0HDgLzaxRyHdniQ9y2jz8zp%2B5bsAkHcUJHvXF8c%2FwAV09exqBhE2AUJ7K7pzxx9f%2B9nEbcI6bqdK%2BD4RaCAtt6MzWxDOjTkPQFQ5TIgoD11JvakeTwRiaOFKnWW1%2FSyn2otjLfJIGbZXz6%2FHIOXMvFyFto%2BnwwO653BqLQZGs4bWMS2xLwth6tQJ4a4jQRqT4Y9QI%2BR8nljkxei49Gad6%2Bu6YmzUr1Swsa2yqIv3pmDf8yaBpowTfFaPZNtHZeMiRpzM%2BU7k0cwpifuP5LyeTf72cpQPVPm02E%2BABBfB8qMVs8ugQhbE%2BbH2biS9V4dPj2bjp7KqmVXSnSw%2Bj3pUDVu5IV8sz6iIdx7ecZEqXatM2dmdDdu8ncugqtPbEDOzkh9k%2FECo3C%2B3dPoZ39xWxd9AWG7WUyk4TfmS5yy9%2FJzbv07Kwjt1j9SfcLFOIh9cPPdD4Ek3Z58XMezOPhlE97MxIsNUpVEXB1l7E%2FlXJDPefDqaX86rVSACdCRae4I2M%2F%2B22eAs%2FNjmhjCLuK7IBjqkAfWqCZqqDSLYpVCdDkAuvNLQ%2FYjwifrZ4Ojm5kNjYx6LmEbD5pGs%2B4YZnhWaUgAmR99aahZvuhYt6ul5EH4A2vSvkEoVm0Adwa5eZPa4CggR8%2B%2B5GlPpp4kPPLIGpEKkKj27NzQUyayG5V2ey1diIn9sRSOWL0b4IbbVXqBuKuGpjbchoG5uVXANiU1gMDdRd7h47CSZFGJuD0qc7UYQSrS7fXWO&X-Amz-Signature=e8188ec2c84091b38302a17a2aa93709cb18281aaeac27376475c6611f7b8475&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


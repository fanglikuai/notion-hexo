---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVVNWANL%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn%2BXAJqvbPwWV%2Fmk4skdY66zZAq7QnHFaU8pW4vZXVQwIgXR3fuASADqC7%2FGp4ifUus%2FIT%2FHnQUN1J6noDyuwwyxgq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDJqOAWvKuBcgaRAqtircA9G1OwqMQnL0ERYvpmOubbPmNxfiWNBVK1eXt%2FGrBU98SR66bEjS9OmuoIsdM9QWQRy7N05tz4lieZtg7SeaAq48MjyD6Y8E9ve62o5PymQeu1CBIO5QAE6dPeWQdF0XOmSLnVC1UjvW%2BpO67V4pNk2JA%2BTNv02kICEwjp31bLqCw2vyADT1iJAfCiX2X7B%2FZ6i1Q6xmlnVC%2FUgKOczzsVsWI%2B0tz9opupslnJrrnbbHW68GlSnavG4bKICptfop%2Bn6EJDE3GpbCCKASZUdlQXLTUrs1ZuXChcwDXO194uf32WZAuJHscbBIcn1BnCCSHuPX0%2Fze5WF%2BBAJ5z6XrbWMDzZVXm1HinhVRE31YPPZ6DnEGRDKUSXbA%2BvIyMC5Om%2FA1u4TBVDfouW92GeC3XlYKhZglfGqhTGBjuW4z5LwmjO7kRQCNSvI9JYaEW2gWrEQGABYpiJMCaFKpSrXzolzAe2Ta1YBDmSwtkqj%2FwOMM4%2BQKTzOufA%2FoQQrT4WR2RJ1Sw4xbOwyh3is53BEcaxJFo8mMuuWop0%2BDbP3MIPwaVDYLM6GHREwpVu9itAnqyO80BA0Nkrb6oP6AkVeM4Q4tjW67H3Agcy%2BBCILOMzeo1pcwgqvoa%2Fx%2BzNO%2BMLTqmckGOqUBNfQpDz3D33zOdnuQ6U6UssqLrRBCZW9WI7G%2FW6A0TJogE03LW%2FORvcvzzJ4g%2FuR9XwoZ5F5KdTImzvuTWQ8VIpyVs5kwdqtse2EmO2NIL7l1drsLk7Y8Es%2F8auWrvT4FstSR8txtdq7vyuOmEZZIWtHVyscmaZlK%2F900aDB8BSUzZ6dO5ixJfn79GNa1GkLmvXVEaWC81kqd4y1VrGhWybBgWfTW&X-Amz-Signature=a30750319cb74b595a7f92b047d627cf150b69a51682089fb9990f2b14d552ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6YKDKKB%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIDBKOYPQ2gdPRurI7jBARQ5oR%2F2yFc7b7mezmMFNQ5W3AiEAwtUb0n5AWhoQlhL74Dobee6hplaOhyXXNn0ucbenpvgqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBThj0sZzanCid2FRSrcA58gcu8%2BIg7hcPEQf8o4Dc5kDqumhx01jC2Fg5DGznZLieZOPgclcecqOiJjjhl8H05Y2dwUBlvMzIbPmOj%2Fx12Ns950i%2Fz4zqLW5jYTK90qZSq0hzEBTQ3EDJbUEwBbFRZ2VK9GzPN4MVEXsYtECQqjYgWuBrlDT7gMrg9l96c%2FZT8GDtiZjKinYJxeWJOgXuW7vu4HdH5WrDaX3dFrN9Lb%2BMtTWv%2BMlvY35qePkM0rAmPQv9wiyvZSUTIeZqD%2Bz2TL7Y7ZMZ7zByPpcpMQBxFi75vYKcbMPOA00bmDHMYS0EKzJb1B1DNg%2FQWTJXn9qQlHMnZV1mof9i182qz%2FdbWBMNJSAIhwav2wDkxDsXWcbCCoASqQIAYDRmDKr6sPQJo6wQlIrckGa93FyZYt9jVfHiiK8elxnh5DeKj9zC44oSr6vFy9bWUPl5r0yi2AK8f6ofvJKtheE5ltgEYMIXALjflCOZO49IZCFm2zOQsXHrSBHmWle1OIU15yWwhW46kIKUg8SUJAW28I1%2F2n4%2BilBJXAdOo4nmuLonfyGD7BRwnw8L4ZY%2F5kdbjN1Qd3VFVExHWqJxCQPNl6ROT2rLSd1W%2BZ2pNCJhXCc2lL7YRVbJBkup%2Br9e9kq9jgMPi%2F2cYGOqUB0Bz4CEXqHJN%2BiReh%2FmEAi%2BMno8juvRW0UK49sMRpXFK7d4cO3XF2oIg10YO8ctlHw3i3HA%2BC4wcYX3IvEqyzRSHSC4NBdK6%2BqLFZejLowOzsW5YPNB4NywfZ5npS0YvrXId9wwaqBYBchfkKUHUINsOsEufg6ruTgIMFPwaWBKXsQZT%2FSeYfbRISWHi4vNcyK3o9oERqsnlKPaL%2FIr1l3hFt%2F%2FAm&X-Amz-Signature=4a69d8ef564e2d6079680691f8e3520ab46e995aedabdf2e360f76849babe3e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


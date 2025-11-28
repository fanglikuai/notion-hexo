---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QYYDIWM%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T110054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHNbAVATw7siWZll%2FuqpKHqHcKy4c%2BvCbE7ay%2BjG%2FsiQIhAIBk9HXtc6wfmNUKenHVDSh6XejY7BfeHViUdjqa3yGeKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxzcZJXOtSK4qmeTcgq3AOtYmnKWIvAQ4xufGhIFu6LlOD46fCVcumLNFtcjBLEOWDEtx4Z44UJxpsu3HNYLil%2BfNK5FUSGrohAyfcyU2VD4Bnb2he0IYlmV7Jt1reoySZ7e0k%2F7K3zO5458yA%2BoEQxgQZ68FxV34l420ff9EkIwhpIFz13YZv6jzC0LKEUnZVdbFE%2FxmUAnvnxCK2xvRhnJ5GjqzRqK%2FatCERDersxKxI7nCtYykOU74I56ElALU7Q2vpWmkQx%2FmFd%2F9559UValqIl92HFQYPmiL5VYv%2BsEKqBlZK24Rh%2BUyKe54ryOegwHNSQICW31Uz19x33cSlQr0fx%2B5c0ohXxk0sKsRO83pLM9rppBcmjezJTWUJ7CmNkPPO09lcOPSgqMEZ2vOTnpdtX8e67aFA4XVYAqfTGH9b6R29V%2BeFkzujsMnVnXKvpVuWF8Ic1oti0ZQxAeAWM3J%2BkIBrmSMepDNgzFdV9mgkIyEe7dWPVKwflvIiHM55pa62SGtFsJrdmnh3otk33gnNPzzO%2FQl%2B%2FERCt%2FD2J7MD36Nq8P3Dn9geR4Tsre%2FbtIbSW3JL9jH3utEUnfjpVNJoY6vGyYgc%2Bzpcfc%2FcbkanAFIPlyrdcmUT%2FK377HAhNvbLFxqAU4GfeWzCe4KXJBjqkAamjKqwnUHa5i3%2B2G2DVmtDU%2F67sWbm%2B6yKlMMCloDQ9UVh5a9xYUm8HnvLl5UoqPNPmnf%2FS5wOGBfRgaSNTw%2BN%2FzB6eueK%2BSb%2B0ueZ8LLveChGHxoork%2Fg274Vv3vKDgp4xfRFI1gLtnqbmi1wsqKSJ6duiSYRH7vCI5Mlq5DxmT6bcFZshSQApY3YNCC7c73d9lbg2ZYLFYr1tYM6WfQ9BFYrj&X-Amz-Signature=c363508ab7b38c873a0640232477ca1bf29d78446618202da42eee5215d19f0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


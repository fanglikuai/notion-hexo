---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QG76GBAX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU1uZ0IaJEf2MfgKuSfYPIbr%2FE5%2BU2gb7GqHJtzIjAAAiEA6j%2B6IwQntTGjrOpycOB%2F8UEIN%2BUlpBPtUV8PrRiZB20qiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMjmeAPw3MNm9m82circA9WESGs7i0AdEFdPB7nOVTP8CK%2FYhVxU9Cy1EpW9b3%2BAh4AmQKDmYFGoOSADt9u%2BgDmF9ib94%2BYJvhljoSC9GA%2F7ZrvA0KPFm8%2BXQbYWVXaws1B1RfAn0GuRBiO9jh%2BAU0UBuRF0MOu2EwKxZzZpKPnlJbw7LBMNpm0pjtCnkNKJMSwIAoG42YHF59RM9acn48UBBAduhBpT7INmCxGlQEWZ9TNLtlupBiaZSjM21LnQKIDFu6i%2B4xQLK41RaUt7AIW4pQZYyQsnJXlMudfc8hTP2aWu2LGOcAp0uE5oQg4rxfdxxqJxxOZ7IFg4yDAe%2FweoIRPgQvCRIvZpTsEZqpnvdp4KqxFD4X%2FELfFS8DthxhZLn9ywGGjezggLb6UAHHBxnQQyMYb0QTTCDeRJCCzyypK9R5YC3rO53blIa9DCGmdBk77aVy86O7bex3CTI69Ib1xHCthgeJ8%2FvM1pOFtkj3Z6mSlnVDKtl7yHVM2dyURTZRfU2Kih80JT8cCLY9adtIcujXIY9fMRKd8vEgJhDDb9bKOzNdlalw1ZAybORGZ3%2BPxuZgTi4ZbiimGJMoHzxaQrxa3UKw8oL7VK8aZYVvkMS0kbX4HT7j0hMnuL%2BKzfCQPaToMvnp62MKb%2B7cgGOqUBGZNLcPZjl%2B8hviQCQXksfu7f1nHdt8EM%2B2AjC%2BcxdJ7X7RwyvJLbnAYjRAsnWFJiak88gZw5%2F9XXRjOJdY9ojFy%2FqNvRXnv6zAbwbWCP85ohywRIoMxQ%2BNn0Xk9Cuw2HWbbBGwc7TFWaFUqZ%2FxfgusbRc9rTca9Y6%2BgqSDUmKNumZsLXI%2BcOJNrk6zqkNeOYMy0oNoUOWGYqSd5m2OCXw5o3%2FAUS&X-Amz-Signature=0ec05c70303781a9697b71ded9a7e037181fd739f94d4baa378d206b4bf9d86e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


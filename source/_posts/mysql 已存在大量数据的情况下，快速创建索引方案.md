---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRCF3OZ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICe0Q%2BvunMWMN9L19MZn9kAlFZzNKRzEZ9R3zAiZ5lsgAiBWyeA1zEeQcK%2B5BdYjCPBNJmRSU89nZxqc8S08WhogZCqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9eawOO4jKwSxb2JQKtwD6dhK4IIdI7ODs08GPxlP04M3D9uBSysrZBc41ktpbHoAmAn0Pf490hd3MoO30Oc2JHl%2F8ZjU1HYp2LqCy2aPwXlUPWZN3bg90hn0xsTLBdLTc6UjETJl7cA25Dut1WT3H97%2B%2BuYHTrfKXYckLtvP%2FdyNpV5YjmLgfoto%2Bob8FvMaOMffG9AC5Ceefz11eJ4Kx%2F1TharPOYDmAV3Dt4TPV2hRvoDtLvd0WUE%2Fnaw95YdeB1aoDw8P7rd8Boa%2Bpua65MP1SYrsXRUd6eV8aKrxt4qN75S6kS3%2BPkMD6vh%2Bvrt6Vkw5clw8Zd5kIrPThxMx5DrQOyZe0DSmfXWm55rxjlx1NK4u6w8Fj3hOL4UomK3JEjbcegVLgOBuJBx58CD2uuRJsW0RgLBjFDScMLzuhcL6BegZ5hmWpFwQpYMiWnPCBEm2K%2BwKFpFgdWs6HuW3T6yRKOpspmeKvlHU15SW6hyUrkucietz%2FwwaeHvMx08QNStNXd17wRJzUZFgix6OmA7wEqQ%2Fna018BxcdCi3O4yqHLnqv8Q8qvi%2FbtlOpMMOjFz%2B%2F2oZaOoeXF341yetvvJNyDTUhOp58IkZeMvmSa693xlIA%2B9AE5CJ7C8tHr%2BUnMIzWCieJISPPo8w5JuJyAY6pgEh81cE6bhX1QgKaCQZljVm%2BHFZNjcvoM%2B2WQ%2BNUi1OesTN067MthV2R8OtpsjftC7LJV1q7dTkltzSvlvWEXfz%2FQvu9XGC3bCGTDvNBliXVIWUr0xqUsu6kIS%2FmYsbMr2G4ppRHPubHz7LduXiWBSwLC5l7fpCwo8TKkbktzuduSKHLnP3pm6%2FwQlI0SwixM0fE7hQUBFGZxwrB4wpgquYpF%2F5yBmt&X-Amz-Signature=10410f155a4d6b68976a9ee589b240c215ce0f5e65a5cbef8838340205221072&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


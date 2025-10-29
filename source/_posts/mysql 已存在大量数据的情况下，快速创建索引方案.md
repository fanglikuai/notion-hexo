---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DP3UDH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHv1SehUpn%2FcGskqQbyKHqK1pELvH3lCzgBijIoKA4qSAiA4OBfhcoZewfOe2r2Fi%2Bc1pwG4sliiV8dM5h9yIleilSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXxhlgCO6kKJO0tx5KtwDom1TPOhXQarc4b1hRMlyrIjgAcAmg5a%2BqF8FI2eCO9Tz%2FR%2FqlTof4CTda75J8uq%2Bu58Z60lRcPyM5k6p3jMhCWswf38XV3vNvlH%2Bx%2BqfqNwfBVPbkJ7fhW2iwawCM0Hvi4QuOrFTbPsq4scEoROkYLTNLDbUyc2mQoR%2BTHNeQxL519D1bYO6LbB20hAS27x0Cf8BBLYsAZ4ViXa%2FITX4r0gGmu6pSiVc06rx34bQe9PAmac5ks2zDAw%2FhkqsVBcpn9VKmToJgRrodPQqT6cQJyHRv66hoKKRQb6I8JB%2BpNCO14I57VnA%2FeOpWevyQfuPvTPOX3tQjH9egG64QjjoVIvc1MQDCb2yLTVgK2L%2B%2FU3b3hoXabKA8lpDd0pVuWMI%2FLGQx38IiK%2BeC5gl7gb7b48Lxp0DQPGiM5dQeOsAySGKa5dq6Zdkmk6lP9DylJCvjrUOj%2FMANp50UldxWrIBqoZhlVB%2FmrEBScPwemufDQpG7ZdLLzFUjTDFjFGanJXgXLHmKE4ZWlF3ZdKdT1Vw8e9x40fwfcIyqhZpYgLenJFlxid4krMrZivrzRbtABaItel%2B0Ri%2F7CGNB%2F0Cf%2B8hEMuOK48tbtoJHsErVQhHZHrm6OtxZn9QUnAUwwsw5JuJyAY6pgFyaOf2H9dHhrAdcZaX%2FA6YfjHkYtCcd2ZmTJ8KoE4IYxy1mtPlfnEWT1A5HlNTK5qlEGJvnW7%2F93GvcLIz59CMlk7Z7AB9xgZH9qVnvF9Ea55lRtvyVleOc%2Fm3yP5tGttUCK%2BP4M1B0MII5Vp3HQQirc5wVhAwGJgmC5EjhUtIz%2F9GbpJ5WB%2BRpa4RHntOrlOB3ikQQQgxXztPMeMYbXBnh68a4Esr&X-Amz-Signature=db56c546b419c3cac8b62eebd5805b7b9502345766fea05971b62e360977a248&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


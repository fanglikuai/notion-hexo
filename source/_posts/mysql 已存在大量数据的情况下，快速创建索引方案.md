---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VL7DEJDO%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGLwAu%2BrfTEjqEU%2BSJw57hSzX4CSspAXKf9XZb8%2FSY32AiAx990Nm45SU0%2Bzp3uolSbSq%2FhaGfhs65vhEpCmSAgTVCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaEiCTHUdAnHGT9BnKtwDBYjzC3%2FJLqa4Cxl23ojHzz7U2T3hppVy4NwIcfjtWd4YDi1bqRKo6et6lOddfwksH1UHA9BhILGg3KwJUtzEC%2FQZOPQg9a1J6V5ombACBAsIUl7we13LxbIIGaRrH6uBG3xEqQjUeujA32qBu0rhhPgf6F%2FpdtHKM800%2FRrv6xFrg0E45eC%2BTRiHYF28chnpnibxOz1d6ijycWYdds4plBhAyglm9K8o9ovorItF2jVDcskMfl%2F0eftiYAchOeymLwEI1WkC1CBQmvXkNC29WduKbsQ9egzx9WFzjmnsXYCyevGpgNp3OtXLcGqCS1vcoqdUHLj1vyVUXc6Z%2FsK4wURU5twK%2Fyc0eVpz4GuUHCsd%2FLsPINPVWbZ6pXSfHADR1wNZZmpKD7IOjaou08G8KCMNvufz0yiehon0KL8oSvdvGMuJ2EbiuxlzuwQRGImypMqoowx9WXU1lZteH5jqMI6lY4clOmZy3QLPCTbOxSCK66G%2FEzoWJB34viqhG%2F%2FvQM%2B71WvZRB4eo8J%2FatqyrjSDjRJe97osAlXS1UDHq4M86KKicX07fIoz1Sz3YWVR1fZaAZ99gnbNgLRXqHuEBvSgJvV6Scxlc7y%2FoC4j7yjPu6cIvcllP0h4Q8Uwl7%2BNxwY6pgHuc3uID2YOgyzlfCcASi1APXoSPYMCULH6blpCGShc%2BaV4mOrZpSO0O2vg51%2B9MSJCtM0lczp6l0BXPrDzBFP6iYiUDWBKh4emcKaQZNvoQ%2BRBSZdOr9Sg4SeFVo14Abkld9EbxMCAvA3eaxMnitz9XJMOtDUGQEAEWBv%2F%2FLx551G5wCF2%2BHHswFPLnQMgnm8WkVwQ6D0qPM1D7%2FrK02BPL4oGQ3gU&X-Amz-Signature=1d7356d631ec8643f73254a0ca55e7d625d5730d4e04deea99879da0eb049535&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


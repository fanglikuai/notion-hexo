---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGUVRUII%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQDKLBHdjKW5i91rzFW6icn5%2B26uSm6GocrookY1Pf2p9AIhALLd4hcCjSzFTMwOz5HVUqt4FL2e4HQAPB1g2gpLCAtGKv8DCBsQABoMNjM3NDIzMTgzODA1Igw1m0fC2PrqrXgSOUYq3ANGcSgrOUyfR1Y72TJeHOQKQSCAbV9QtcMMziIab%2BFEwQVrJQlUvYy1nWLOghPqxzCOumaFUYs2ENzM0X9yNk7jIcXZ3YZaQS%2BsskyWyfki%2B2Yn6ARUsMli8e6q8X9Izf7mQd2ktK56B%2Fs%2BV7JLMVBJDqKZI5t0ayCtdzt7JW%2FcykbUcXGR%2Bjehasdv4hrwH1va7XHwMPhIxhg%2B%2B4EdcKJE8fwYudIniHCx9xPzzptZE2lG%2F9PhBEmNiMCQbLCnRNdR0b%2BwJfbjZqgHCWg3uqq3QvOo0yx8yXugd21m35KTPf%2B%2BaEBe8gl7TJZ9mOJGzqUu9qngD7nmEy0DUSkoiKXjjmUsNlbpRuIm0Ms3PqRBbWjB8R1FUADcv8fuzzovK0sKAqbjcFlE%2F1RSZN7g2oVwcCs8b9D05kk3AP8OxalmGNZp7rcTORKlvu1xqqxsu%2FTk3SiCEf3STYtqebucJHEqHfCt5kcZGiqQL%2FaUfCf5PzCz2OdXGSYqc6YW3vlL%2B%2FWjcYrILvmikh6i4sdhdS72Yb%2BxBPyhWy5gja%2BflTgffAjGJyPwI3I9R%2BKcgZu9BhhdkGRD%2Ff3ugSlZiPZbJgM1V%2BuYx5pVFaRZr%2Fo8OiBhgQs2GYx38ToTlzJnkzDRk9%2FHBjqkAYeiEqBQhR5qVYLuGbJiy6u%2B%2FfUwHds4bC%2F521ot3VpkxZdTI8Zko8bUImn2uq9Ar0ALCqWbCKSHsR1IdsHzveAcA%2F2s0C4Sdy4lXKdyhb48hSlhtT3izZYyQN21BKzjbLNvVZTBOTiECo7eYsGcapBb7P5dalMJOvAibbB2DG3nrHl2%2Bv6EKfgobBKvrF%2BUUBngbPKOGR78Epa5pzmX7y9fpemv&X-Amz-Signature=52c88dcc497c5ae986b7e2245493ced04048dfeee508cfb579dc3700b0982418&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


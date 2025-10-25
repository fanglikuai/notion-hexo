---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GRJB5ZT%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T100052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3FhPLgZGVo%2BVS5VxS1AEaxDeu%2FA6gz2I4%2FzhZ86GbiAIhAPZF8YYUIiEDsKrFy3lHAv01IGCYsEDvHPUJeNfDPCbJKv8DCHAQABoMNjM3NDIzMTgzODA1IgyHpI%2FZeOMzCX1W%2BsEq3AMGzPhOJMjK1bg1SgAyfE0Fv8sbKkGKs2iAB4BIGj36biLAxXqvz4LVKdyttwxFri7F3ymb6oYtu%2Fc3CCZNvsbZLOlkSyGbvs%2FFskI%2BwqamxuPYhF7g8pF6cS%2BRTQjclMAwZQ2VTZx9AGVWMbBOWOgRj%2Bu6Bzlceo7CsaCnGUILFyX%2Fu%2B2M1VBhmq52UYGlcMnw9oCIvd%2F9meXkhFZAHkGwP6TGQpLmjKTF1VNdixdRjgnC7cfQ3%2BbKAY6AuzJvetjcpsI38lkF7edsKMq1OQ29qztYQBK8%2BDdGYwAxVTC2Ks15Zz09RZssArwpgjhqtLyZ2xFMJeydWHk5Ls5K8dHEozxAJ7j%2BgGYfPQZI%2FAhjWRd4BblMe7BJ9%2FHFARYQQ9zEbkj0dCkvgSF2FGYgnfiBdV93DPrXzQJ3EpS2QEt72DguziSwfQbHMYtpcHA0ncUatZcFWD4KQJRVnGnkO2i0y7kqk0ff9QDdVFRCo%2FAOcBMOdZew9AJXbVKv%2FwTtJfHwMe0OT7gyv5QZvehBRcBdlRbmKKk87D8Grrzg%2FI7LeTqne0%2Bx88gpj61f1DTiJIo%2FKnXdABaEh3TnVkWXk3A5S0i0A5nvnqDah7XgfAJoZtf0dfqTsA%2Fx4k4eGTDn6vHHBjqkAdkfH1khIlFzmN7QCmjzwuxVCqNrpD14MWb5Tm1tuIGUdxd9gdaNA2B2VB2PCtwasuSjq6Z%2FssMdNblyQsBmeskG59UlNZQ7HmHk6Kt9YYxjk6yDbsQN3WssAKrSZL9PYGiQw5qNSqGJA5LRVwxVkm5fja2UbJFY8Te0AuAHizoafn8rVT3mfgLzla1qQfDjKwGO2D0RzlGFwnW17s8bVgiEYmrz&X-Amz-Signature=4771251abb61b3d5ebff041e0f597d8af7ca2c4c01f955398b2af7f4dd8e213c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


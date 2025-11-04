---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBMEVOY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKmZwEBHTBaiZO%2Fs3tGmcP4IfNqJY1OQh5mn%2Bl1oHweAiA50QDpHPknDvVovbu1DAUi9MjKMHEfmM3AJ7xc4yuB%2BCr%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMNRKgj3poxFXH%2BDHJKtwDGq1AnMtLNH%2BrYhun2%2Fq%2BAX2UATbSFp%2Bq1hUY%2B0kP3miZs50XIRkhnoAO%2FU2VnO1YPYL%2FgkV9cskU4JFjIwL73tCXTltxQQFP1oKiTzAdPAVVr%2BsCZs6k5s%2FJyKsX6tvwZPA4KSZXJBQZY1CoGZ8ZR7neaW8hYj7TrV96d4AkQRUcFSE36y5MJlS15SxOTbS0xLP0JjQuoGm4n0Vu%2Fg7gNHkRXjAbmXNJWoO0NT5JO52u6h1b7EQ9NbSPj0gFOV%2F2ZmP9BnXijr48HWegHzlHD1uW38j9mqQWNMGYoRnI9kw3Ja7746qfVzUqS%2B07EJqeF%2F4AteWrrqMQ2%2BHNTsH%2FdMDO6OQMQp2r4AXtcs9nRa0xL7HTDhWyosNxseZvSG807DWOMjF2zbeti6Nreizc1CC3emSpk6uJCDnOHkDJ5w5J1nljAuQqqoz89oZSVT1IrBiNchP7eYKrPpmA0z3%2B0GB%2BP75Vt2khGbzLKRnYoS0ZEAnmJV1FxasLtdnBoWMGnXgyD9SIAMNKhtfjwlIednKzpM5csyljKwhMfi6RnPWmxcRnU1WuZZ8%2BYTNnt%2BaB75Kz7HGjf5JcJXpsCAg6QKDJk3SFunjle1bPtwpxxEhGm75%2F6V33LH3nXOAwwJWoyAY6pgHtOE6s5XbI1p%2BcphQfFH4lh855oszNlEAxN9lnL5KtpPKfC0VNDLho6pe97s0FQQ8DWu%2FklHK8k2SLF5%2BguJzA1XWv94rz4To4vjqGQySdc3bK%2B1arZcD%2FFBVj8LiqkmNs%2FGebNfJQrxSI%2F6i9MnqJwB4mHDDzWcxWv9pCFVYjxuAmQPyTXWcyd68Sk8qi%2FL9Jyb5gIoDM9eGE8zJKdE4glJodfV0O&X-Amz-Signature=8b575428238c737d1fe1ec1bf993cff8721b23143103c5d1f38777177e3334b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


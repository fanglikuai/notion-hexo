---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBMEVOY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKmZwEBHTBaiZO%2Fs3tGmcP4IfNqJY1OQh5mn%2Bl1oHweAiA50QDpHPknDvVovbu1DAUi9MjKMHEfmM3AJ7xc4yuB%2BCr%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMNRKgj3poxFXH%2BDHJKtwDGq1AnMtLNH%2BrYhun2%2Fq%2BAX2UATbSFp%2Bq1hUY%2B0kP3miZs50XIRkhnoAO%2FU2VnO1YPYL%2FgkV9cskU4JFjIwL73tCXTltxQQFP1oKiTzAdPAVVr%2BsCZs6k5s%2FJyKsX6tvwZPA4KSZXJBQZY1CoGZ8ZR7neaW8hYj7TrV96d4AkQRUcFSE36y5MJlS15SxOTbS0xLP0JjQuoGm4n0Vu%2Fg7gNHkRXjAbmXNJWoO0NT5JO52u6h1b7EQ9NbSPj0gFOV%2F2ZmP9BnXijr48HWegHzlHD1uW38j9mqQWNMGYoRnI9kw3Ja7746qfVzUqS%2B07EJqeF%2F4AteWrrqMQ2%2BHNTsH%2FdMDO6OQMQp2r4AXtcs9nRa0xL7HTDhWyosNxseZvSG807DWOMjF2zbeti6Nreizc1CC3emSpk6uJCDnOHkDJ5w5J1nljAuQqqoz89oZSVT1IrBiNchP7eYKrPpmA0z3%2B0GB%2BP75Vt2khGbzLKRnYoS0ZEAnmJV1FxasLtdnBoWMGnXgyD9SIAMNKhtfjwlIednKzpM5csyljKwhMfi6RnPWmxcRnU1WuZZ8%2BYTNnt%2BaB75Kz7HGjf5JcJXpsCAg6QKDJk3SFunjle1bPtwpxxEhGm75%2F6V33LH3nXOAwwJWoyAY6pgHtOE6s5XbI1p%2BcphQfFH4lh855oszNlEAxN9lnL5KtpPKfC0VNDLho6pe97s0FQQ8DWu%2FklHK8k2SLF5%2BguJzA1XWv94rz4To4vjqGQySdc3bK%2B1arZcD%2FFBVj8LiqkmNs%2FGebNfJQrxSI%2F6i9MnqJwB4mHDDzWcxWv9pCFVYjxuAmQPyTXWcyd68Sk8qi%2FL9Jyb5gIoDM9eGE8zJKdE4glJodfV0O&X-Amz-Signature=1599f3e67d345e1429f0b72992d2068cd7c971755328f567b7843f6bfbe1effb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


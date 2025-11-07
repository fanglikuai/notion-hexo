---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY6SXT33%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7yzze6knNx9BfcEwBXbPgq%2BhAbGkgj4knixG1%2BnxyHQIhAKQE5s5ccohyAap0CnlDe6Z3d3%2BtxBCmwYUQ15aZ%2B%2ByUKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww5J2eyFxDTU7vPOcq3APltRwR0C7%2BFeqEjjDol1u7T0jA2LSO%2FOqIhuKv5ubBhJ5nk6yNJt07Ajd84XjuS8rBqWeC9JroSU4%2FgBXBkKqXc665RFjGFBn9Uo%2Bf4ajNDcyvImUPKHfx0iBfe7OxHALp2%2FOKDN%2B3JUHd5eJA94O2LGmYHQfSH596474iZME1AW4ZT9O6Q1AKwtziVBZm2nA%2BK4GZjPl3dZl8SYS9rWyI3n7TAsSBzB86B3jAn0BYP9pg6kDI5f1iWBQ02XPnzkptgHx0BwfHRk2pXJm8hENBZ%2BdsEUNv6gsG89wRml1l8ssP2dojkK97%2FYxQDQ5%2B8qeroX%2FF%2BY9RoWJmrswPyMnU88nReSuPVqHsCjawa5vA8%2BYbVmrhL9B2zTCbgZbNC8jXztNmaDvrnLC6%2ByTydQ320JhU7abDBqzNQvRS7NVcHo%2F2WJZYrxYUFZcmy4U4EMYpiow7c6Yf3DJ9PEHz1EgRKrPXcPlicz8UEfunGc0aNy9nqUhpYPoqnDA0mKew0iGSQAwxdKSD%2FiluBEpU1tG6eXFqkzxWNcB%2FZ0nRk9LWnUmPQSmxeKvd2hVARZPz25OHJSXD%2FbrIe0zenUsMu7AwJksr8OOnIqvSGUmhwXdoh9KHcNh%2B4p38EcCZ7TC%2F67bIBjqkAVH77C%2FAP91cuEHZVyEVOGNRThMbVHm941jJy9VOXdBwW1c34YdtF%2B%2BrvqFPWAbBzqBYfWDh6EWr60RfhBAi9JRCDpp7U%2FQ1C%2BI0QENjtm2DIG%2FkAG0r5Tetbmi07TK1PM1WSP6UKirvMExG0hZAUxiM54sANZtgpyqim7GVS%2FCYcH1WA4OUVaC8vfKPpAooSsgQPhlOLgfI8wn4tUsdKLIdcuaG&X-Amz-Signature=7f03aa83c8d9f49758637723c80fa0fece02433d172c2bbab688ac2f67675186&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


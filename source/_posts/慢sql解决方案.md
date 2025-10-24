---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ACUSTSC%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC7tQHiUK7pC%2BC4MBiBX9qrbxtj2AV%2B0yNePvZhP9dkkAiBufBdsABck5nW3eN5RpnUgMSF9W7G41HJsGUwFeaeS5ir%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMZGc764a6V4PRcbC8KtwD1Cg%2BVs5ZAhIw4%2BlYqVSK%2FSnsmyszyUGHKF3Vp6LKQ1E5bYjgCT0vF3h%2FDlUzIp2pCPEmlxfwdQpVK1FSBj3Aa0NNeoChisGhGGvYK9k9TQm4Xw4eWKsF4vAZk%2BLU7N5sC6qFvGM1DfBx959iBIJW7GO3K%2BQ%2FiLTyOxDm35vQg0qm%2F6ToEAGMyx83Y7krMZwufMVPAuSqdGGaZaRTPqeWnmNIqEkFtnK%2BQG2Gcy9aHcYFMe7VrYHbWPoUr0E1zKqNYJMmUoT%2BucS8C%2F7YeA5XIMk00rCqRCt1RKrX7kJaLfeTNH94P0%2FGUhQ0twB3RMg%2FtrXb3NaNJYky33HOFMbZwZPVOR6vi4JRaThyx7AaKwavzJcmqyxYq8CtOxYOKIRMr%2B7o6MbpYjDdVcSlQsySjyllyCR%2Bszq%2BXJjt%2F8D0B0Ycm%2BxAPVy4sTkxIcnbBWTHhKIUOHYuHzCkyG7xTPncuI113cWWmcKad6nINWsY9a%2Feo3i108rPjLuX578d91eufGrnIHgtt%2F91yROUynsaPGVvt2L1vZBq4XWhfSpCMUq5ZxWgd%2B88xUEsAvO6ZnEcZrnkH9eFRm7CW5ybDhIIIUtOR7XSXYcWfAFVywepwtFOL43%2BNRUfvTktRsMwjMftxwY6pgFz10eIDl6DQzncsHFqyRsnpe3C3J2NOqyY82QbdwDn%2Fbt6MZOzdYAJfNVwvB7f7pld0iAIzgWNG8uCy6uYeoJ4gltM2yfJSPKTR9CH3PSgE4wEHvexb02mavqMnWb5RYx4CgZDf9W4mxVA7q5Nye%2F0iG423U0iCgrr0QN542btkLQmEaF73PzQbYvpwo1vjUcuRifVvg%2BfhWQDs2cRMn0ce2I4%2FLW8&X-Amz-Signature=3eb95775e975dc8faac9202d6efd766c0c8abf8eb6eb0817656764c8514eedb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


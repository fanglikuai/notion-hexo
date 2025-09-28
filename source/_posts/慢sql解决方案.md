---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPNHRJLS%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIAq17mbXBT2%2FMh4FXf%2Bd480WBjuJoVjb0RB4bvTaAU37AiEAnxNlqfc0Udu8G4CmaWPVv8imgsKpc7vT4Rt%2BEb3SeUsqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAHk0%2FZWvGf%2BfVlyhSrcAwEpJZiZOW3yOYL3Qc3QhUHBjuqtmwJ6OeeJ%2BQV%2FQD6l0l4CDA4DW6l4OnmSJ3kg2Ri%2BTzKWgjP2otBvUI39qYlgfuqud2%2F3yqobMY1NPm4PolKhVRWp%2Fs9xNs%2FAIIukyg9jxvWaMr%2BMd%2F3OWPusdN1EQ9uA0LQ4GrnX9S80qpUC81GsEWCs2sI3m0mkOnrdMQaZFgTkv4m3GvuOWIiLr7x1wJscKHODKWAJXC0%2BbtMsrsCxDqQIcwQ3SMZl31CTr7LQYxl5JXvoX8gpbGEENdaJSv89arP43gIJyArbxdybDeutYeAMq8kAaX45uOAzLUeoXnlYrGQ4zF9FXTF5fyGHzQW%2BH0GAnQDeIwEMVb%2FZYgvlDNi6XF4KO8TagEoi1mew7qOlagm%2FL90IL0Mtz%2B%2BjX%2FIDGluZFARRGQSgZypz%2BpWlWIlt28BZGPyAH5cetxgQWBpU%2B%2FJP2ugWlw7LgQpAWXbFp5Ub9xvynupUapv9xGtTIG1yfQYr776K6IEV8NkXBG2z%2F8RjBSzqRuWF6iDpZs8a0s259Rd8Y%2B9fdbQgkD0uyQQHlxLyLVFTY5PIo6nVTmNB8b%2FU8mYkKn75UTvBEG9Xmh3BSoqXUoRM3fnSIpCd%2BHBeWy4wYJluMI%2B948YGOqUBTxlQtdoTv9yESLejWTgU39PxiJNtpVze2EFT5Yr6iRDhxwGCpe%2BPzI0ZA%2BRNC7noQ8san3rrW0lX3TtmQukt8vOT3fE3j8QGHBbmZMqmzxWiC%2Fx4a5vcjnwDaI7GjFeYQm%2Fh6C6B8%2BGDB9WMb9jdBq3AXJzt3i44Uuil09jUAqclfww5HNVwa6AnfYRl2I9hsTJCrS0cust1V8yGLjYabNn6uZGO&X-Amz-Signature=03a067f7ea4ac4c4ba82f65a546b2f49ea46073999d721601e232cbc7e737f8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


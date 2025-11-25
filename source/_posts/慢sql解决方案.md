---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4IZ3IF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAyZmNJ%2Bwqo7Cd6GhEvI8EvGb7ZOXV8wcDyPxzIRXdw4AiEAuLMy8CcJl0JOZJjo20YL6%2Fa8u5owmdA1QZdLfVNT%2Flkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDPjo6%2BJQCFVbzh2qECrcA874dUElUaeKjCAqNv%2FCTZgOGIzvtr52VHavvrsylsgNRmnvcWxiQBfHahOKShD1Zljdp9eqVlS%2F5lNtdmuShOa7%2F7yq92bmJm3Zwn3ndX1VIng8Nw6Kd5IrYuZCSY5Z9lx6Jp1qrMRjbvekmTBfGW2ZJRHla%2B4nXdV32Jyb9alD3lG1mjTlh3wWNL%2FSiyIFEOa933CSnU%2BZ9HreTgfbfvlFR3xW%2BgY4F4JwqTCy0wZvphs33NUvDTX%2B6gx5zut8BqgCTLnHe1ksaQTtu1SNbsFy4cbjI3hsCNyKFeU7hlRYkOJmfcVfEoD2ve8NyVHPLI5Hm%2F7b44xbNIEHqdS5uC89klLmJEP7mcXO3iSzLAUcK9DYh6u9EWMiRQZ0L8aFQFvFg3TaSAQ2HtCOV39MMgOyK89Z59PkrDf0Rfldu8wkLs%2Bsl8XB5hVhqZQy6nTi8Rt6I48Q%2FOfaY1frKIuZps2iuXL2pVrBPyP1uAYS52CZy%2F0CxMzg%2BV%2Br%2BkYvjMDy3nOC143%2Fh%2Bh%2BbvuWGQ%2BsDLwfCNKThYQfPB6XjBtJCNRX1BkaJnj1yhXxG0qMpCSsHI00tqqMyfYqWREHT%2Fiubi8uOzKFHgazDIOv52Gqr3Ug84Tc1DiEB83jXJmJMOe5l8kGOqUB%2BVLMgN5dkXNOHWPdhV7s1WZXEXZk789%2BlQTScohonItLtwTR4%2F06Kwr%2Fpey5y8iGoB1FQjs0g9qlEofmhBmGCtl0AzeqfmyqJ9BUZI3Q2J61x4YkI2TOxmVHFzmPUtPzBxRvEby445xOxSja5XpnPhwH8nEA%2F3aC0THp8Hz4HF2DS6O9dZOIRMBDO3fFiuniXBoCwR%2FFWSaJdcWWvtNrMggpydGr&X-Amz-Signature=89c01213abc798df1f0af14fb4ed0ab263e078a8cd0af7f4942153d19f80a7c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


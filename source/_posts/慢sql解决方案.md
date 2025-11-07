---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662H4T5T7E%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSTmXwilsqV1d9Cgl3gKJO3eKVIab9stTWwIkNE0YqxgIhANQ9FTMEEZVkrR%2F94ueiv7OHkmBGjH5lPFmLZxDko%2Fo2KogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BbzV2HJ8lmtSK6Wwq3ANoMukOErDlSgFvt%2Br%2FaryCd7NnR9Tg9ThtBW6WpJKi%2BgU6FD%2BMPHUOXP0vHU28hEI%2FiveC2O7UMcIriNgBzUPw6l%2FthwINZ0Gf1c9Jm80aKlSnHo9xcw5eBcVD%2F%2FLf7Jy8cyaPxIEIZAi%2F%2Fu2rlzALZyoG%2FD7EgcEZKnJJfyU4pjfnHgps0870cNETOYD7HqMAmjyUEDWQZ1%2BwmN2KtMSxXaxMLYemdV%2FdyLPTP%2BCUY3WHFB%2BJxYqIiM2Ojg2g7PHzArBBNjV0jLXFsY0q%2BYfPMKZpD3kB6LjQaM8Ek2Gb7pzybHr4CLraWr67BTbLo9rXWoFZgCDyYK06VrCF42kFFWC4S5hfqThIYb1RBd812leC4xzmFSmTiiIWAQ6hqIDWC7n4OM3Dv%2FfV%2BtSelknlEQlcJc7PZ%2F6X0qx5rml6Fninn3xPtDIb0LQar88%2BuM%2B6SfUcyMI4LA0%2BUIQCXjrvhxHu2dnxSQD1eFDdmJuWcDqBroinA1CpckBN949dnZMGNKUXOFCFO8UFRxOC4%2B7OKdpjfXXPGoo9BpqlNgJlNGiRf3pWiePQBiOx3nyyvcjLqFZUHdtysnRilgMcnQKgTDWO5JdQ2ZqY1CcVV3yeCDwpZLb9qpZCtgF1oDCY%2B7XIBjqkAXuc6KuDGWuggrh2zoYst7%2FzpN9YkdONCbwBHSwrGhKNj6Q42AISlxCbr3j%2FjyTylElrWUghHS9tqaTIJDHgoBlWyF1zytPx1KpXRQ4nwUTVhc0%2BFEBzN3GiNGYkxaI6CO5ABJzFqLba6HSc3ubLhnsU7k2SJ2S0CR7uMEhwUG8IXgjMTs1I3rwK6uVBgoXJht2JOcyKrfS7TYO1ZU%2Fri5y3lwie&X-Amz-Signature=128b9eadef25e71a98d8494b2354329503f5630f343257673ad1cd55b3d768a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


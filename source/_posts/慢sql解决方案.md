---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIFVLTBD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRM2FZK7moBQJXSwX2%2BTp4IfPDkig3dm8trRtMhWyMwAiAGmJoxJ9R%2B1%2FG8suTw6Lu1SxiyVm67enoTLm0kFO56yyqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKH5IAgDjKsa3kXr2KtwDaQQLh1RzUlIz7nU8gtGZm%2BNzt89gPTA8GxPhII0q6CpirCRsNl%2BCgNz1nDFrKAwnZT6YHEIPpOr42BXlroFV1Hha2NOjL67RowjxVb60NFkIogwKEsxmhJ7Q3kDeCMa0629BOCcULxz3eomvn%2FGvek0LtJTXJwfEY1WG3cjJCJ3aXdfWCvGZ7aS5TS7dhVhR1Bm8HtR%2F08PzWMX7lf49sSIAmBaJP5hqpXYO47zhE5PGqf6rSINNhfFbV3Z2divFsgB9GW6uVIoqnRb5Cxi5lUsLHZf4nkXgeqqW68y3tzarODd%2FT112FIeThZj59PZV5KMWhu9Roe5ndCybgUjAzOfqyVlDbfk98QvNdGRcLg1kRVZJFxFYK7mB%2FLHfiQ%2B60KOlfPKSnzNy%2BMn6HNXkJEdj%2BrRyjOqkiZCdj%2BhQQBE%2Fs1Zylj%2FWGqBy%2FoM6sIiGryJHJFNwnnJ%2B3THyeOyKGBpdH%2FZtxbkMwrwOyRumL1q6v%2BIndap2f6oFFF7nNK%2BNnXDVouxbo1107hQih56jz13cD5rQvs56y%2FmlbmcRXJPhMCGCq06IBQjOwhEmcS4k84cQdrdollGw5yXlkZNZALUJw37XLq3DNr79EwcYq9BR1hhZQ%2FtFn%2B49ZbowpYyvyAY6pgHbZKduwv0cAzqgt5nSu4mmusmg7DLdLnQxRZc8S75cFR4LHECy47xnS0y%2BPzQXN5AwURVnluvV6HQS4lylscDB6sCX8oooarYho5RboX5nonGvhqb1L2km8Qjf2vdUCxLGaCPGlnAEY2dw%2FKf6PI1iXjKL2TSNui6VXmavvGRO4ZwaQ3mqkoZ%2Fze6Zs%2BENYHkc8OqKxcxFUDRMLMMULHa8BK5yub5Q&X-Amz-Signature=93cb8592db619e8764dd645d35c89063f7c859ffd00467d78c43193143a86508&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


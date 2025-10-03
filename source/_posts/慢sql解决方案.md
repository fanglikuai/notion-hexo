---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYC4B6GI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFlC0KXWScsf2Ze67Eph7tro%2BQc9v9A16eML2xBDolciAiEA3nhT5VCvVNX0SQYgR42bAnQNe6gLeaTLj5vsa1bSfmwq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDBgwCWT%2FZ%2F%2FKzv2GSircAwQm5jaaYWkFtOnahW9kGcxQPBwIoXsC8wnwVn1ISj6bHIKkqXt6QXbsYsGf7YUYM2WlBteO9My%2FjV%2F%2FP38jWLMR7sW4pyJaRM7cvT%2FZdG3hZvx5crOpwelETw9LYorNgCqFuafXUayDNPlF89uOy6%2ByWlh1mCASMxw%2F3uuahS159xSyragckKWd8AHoPnmsudhWUo5880jlW5zCaWoawQyUiz561%2FCX733tPz%2BvT%2BE0xW7EVixoYoKlVDZCFSyubok74YyhjvM6M8ZNl9Fz6hs%2FAP7BNiUprNBWir1bEg7%2BVyoJCKb%2BWx9PItctdUp4aaq%2FX%2FW3syq7uXpq0ME5pPfnCScB4rD3%2Fn3lIvayjtkbB6GZiciG08LzStnx4GM3xKcXhZj62WTKmG8EFpk5FQoS8nGNYBqgW5s7Zwb6YR6lkDVysiUL7RH33h7tOIH%2BEnjwFq4GsMG0BYy7rNWe%2BncDAFZp2behldogc0TeQpvu1%2B6Mk3imFJIqVNMVwa%2FUJpEKuPX6ubjT0I%2FgAaISkMuwxy%2BWwe1GF8kE6jhdZYaNZTxFVNIY%2FFcLPBScGoFLADrGd9jSn1h9PkxgOD45MbWykVVeW3jb5SlO1NC69LBcAnplksJ0GGb7wdt7MP6H%2FsYGOqUB4OdiRArhAKksy1f1meP0ytJ19HzhcCPaM0cc48PchZDsgGSn2%2FLSMR4Pr8aaZX6vArr2pmQ4hq2RbCHkKDK2MCYB1rhSGFCFeuBdJPO8Os3HazvYIUXXr7I8%2BUX1lYX%2FN%2FP0ACvBnQDwfVJ4Skb2%2FdU5Fe2C%2BaTk71nrHhfZN0HBJqTcjeInkUo2jlt1DEeyw1L8yEYfVikxBt0IpCYA6zVdZPhB&X-Amz-Signature=abf1a5a9937283d3649957b2beaa40c59cf8482d2e05bd51175acb4a531c3326&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVF5FZLX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T150119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJGMEQCIAFtl8Axq7vH1TLcP8WvBt3umu6tT4nM5YYO94LqZbBJAiBPY98jLuzPzO8pDvP4pGWsq2Jvh4Tct9%2Begrxk0MtcgSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCV7Z99yjNQTnZFxTKtwDKmk%2F6RsQGsEDGSaqd9zce%2FIAUo7UCiZoqGJ6jA5lPoYbxxUKxLhNqNGVob4sFvexf7kVZoZWNqmhY46v1W%2BCNL2bCtFRN6y3g%2FeBoa3lPK0y6niytcv6gHkxYnvm1tBfQs68yZNcrfpUbHJxDYffOH4qy4nNXjqQka0Go%2FLO7G80WHIPjNYw7Jqxkft6IoOiwvPYzPwyeMqZ3cfPbAXCc2CDmMRCPdEtVOjqOum57JS7dO5nk%2Fdhs9QYQUxj91cEmhsdDfUAv3ROqRzl6SlFj6EBSTyVBu5%2FgDW%2FrKQrMQ%2Bu1A8JATnWjrhHsbKKSI%2BdoERBcxh8sFxt5MTq2Ft4rz%2FIS19hJd6a31FSIE6LZDXjIg7xsuc18shjLF3MHkO6h0NR3U3rrqMHRlfHR4GgDigmUo4Xv6fLn1m1CS9QDXs4T8El%2FUFu523z690cmDOB8rt%2BNRzGsp9b7wvNTZ3ln0R9aXYJD4ard45uWf7yO5207tduF%2FO%2B3Mt%2Fpfh%2FQXbGQIACSoyx3lcdENvCJ%2F4z%2BJ3SFONHjghitqYbtoDI%2BkNMTGacTXsOCmmhxcSOJA0BHVMqo8%2FnNHq83GcpWHnsK%2FJe8GL0%2FDovQUY8yQMpRaaEywzLh2ZwQomh8GUwvaakxwY6pgHJwWhMFata47EpTpss3rV4rDgtjUxMtz7BpAr8X%2BLIw1k982AstW1CqLY6qwM3P3vrv3xv0H5B72Z%2BFtq8s3RFUwjhqHrTuCQGprvbiZ6%2FKbbvDJNN52YrNta6zelml9GeKOmFBq1g3TwVtBOHynTdyOsphJjVNyjn8%2FU2ve7RBGYIEEIkNwi7NvSUHtbWyEofasjvPfHpN%2FTA32wMwfZFaCup95Gg&X-Amz-Signature=19ba34ce589c2a8c54e0cbe17b78d886fdf3b1e576781de94b5d1a2868ba4f16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KORLGAU%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCICLV9sjuVDGRECIwfIrWHHsR1lfUa5B551%2BETihFkL28AiBcurPLWel4osT0AszrNJu9I%2BFZH7Uwro2tVdNOIgytDSqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7soKfVXiQ5MrL4JxKtwDazwQ0pQ%2BIoEcj9jIHV5bF1Ex5cjjTb5xbh%2F%2F2d0yX9EZuNVpcEoNt1KORrNnT9O%2BpVlTfosROAABTLeAm7coJ1Y2S5fIbrUAk3ShXziEJsJtsfUJvnbKtLd0MvC1yw6T2%2BqZ1Ry3aDFnuyLUwR4OtYlua2dpPOwS1HPr%2FA9KKcj0eOGjtxob7e3Fih4rpK4bI5YD5%2BCvFqtJA0psc0FuRvikbdDaJJ%2BCAkJGRxVopiPKj4GGjrFfO9WrZTOvIUQkrVvX9fNo4qRWpOmvD4sxYwjyHACuJ2ZQ8bidWxDeyuQTJ5%2FRml9FW%2B8J%2FHaiMQ1fpH%2FBEWVZ6XpK72B1tMwmQLW6I%2FHBHztUUMu73bY2FxJFq3%2F73h2fX8NZvY0TqXb5Kuy0PzIZ24ZC9aIJY04PeKrrJt1%2F2xG9Uz1cgXtoUEVPiCspdJIB0B4p1S82zKrx04hWxhygAVYBLHzeob36D%2F%2FG2yBRgbvGIL2mWsiZwvUsMnb6tK6ytzVJS%2FPk6gN55BEeiVZYnPRbpZ9WRSgbcZ%2FqdWOWEZwamkzSfmNDrPa7Wa%2BcxLpGDamo1C9m51aTIDLU%2B8v1ZvGisbPmud11Ra4WeWONvOpTYPUht6xwXxJpXad5WOsm3IdeUnIw3qOnxwY6pgHMvKEtkW5XzWDhoaZu5myRHJ3KWgPZSkdFkvTvcbUA1tJhaVDTM3jgD3OLRtUUjTlSZbk0q96P2IBbAZRwTlt7rfODinDLdyGwoDO1UGsDJapxkSbRtnBsdO%2BMbjV6Hlqprs5spEmdX0LhKmc2CR5w%2BLvna2bWR%2F8C1Dgvy8LPrhtW%2BP%2FKsx9uUnVKadDNolSzdu%2BDWksvBamzbP6Bnxa%2FE%2F%2FIXVds&X-Amz-Signature=416cd977be3db4583df9edf3aa7477eac49268a3ba791c96432c717c83f5f6aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


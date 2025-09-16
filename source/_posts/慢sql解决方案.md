---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NS4BNMB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDizhGb5SnEUhJBSW%2BHrIVpYDNueJ7nER%2FHsTmkT%2F07hAIgdjBpU0vo61KmlyHvuJLxjXLjjtJSuXAA0ShmZCTfam4qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDrdOI6oLQ%2Fr2w%2FKVSrcA7kRm3akmp5NwsF67MjNL%2BhU6eNUEp139Tq2nBAe378YXcbhzKTUjExGdDqy9Ir0Z%2FkCCB2jIBklSiQf2Ple9HNtP6EMLwU3ztdR8IEujD31A2Un1IxgDhpkoiIpf9K9C3lAqBiNqoIW7YTBy1D3pDdpZ%2FkqT6yAocU7b6TJDuWYzNCYPDHmcBoJZ7NUyNfQGTv4EMPHawerasnuD4DankhWurKl68TfFSd7UjnexV45LL1hhBE3C0Sr2hHx9Slfu3JobkIep8dinTV27ttnH5E%2B1BQFMcl8y1Srf1iyLxD%2BXJ5goeP1eCuX1LgMKl%2Boa5e4sn4NAGpWAnfxZUHh4OmZXp9wyM3gMccK%2FyHCZq%2F8JzeBYOqisEH2KTbmIUqSeo4HTVuHCLj0IT57uygD47%2FdEN4UBrx64sx%2BC%2FaSlhcKm%2BBR%2F1gSSSlDNPZDAfg2EkFiq12iXCiUhIeJD2Q41t1ePAJ4hNsGHCRL9JhScFS1V%2FiSko3Uaj4TZBrsyxLArHC8vtBTue4y4fG0vQfZQ5UDi16M%2FazFFDOHnc469p9cMW6c%2FyUzs%2Bbm9aDuZaK6pEkBP1mISXLbzaDtC8rWSpU5kecJcmpBmarKLLHP7Vtx2kGBxfC7m0w4fU%2BmMIzDpsYGOqUBsSonoYimvFQCUXS8ArkwvZGPpADwki%2B60Snv%2Ftujud5r%2BCRyJTskCk%2BZfGSsRq0lzhBs89QLzTrrjY%2B2c49C8Ix94b90KkSBSSh97RfaVmOEweZhFg9AojJqXZdTPEvayH%2By03ZALw7JAbu1RtTrCHm1D5hG7eypqRUo2luKw7Bbb%2F%2FmmSoISbHxgjSgTHJ1%2BXPJFH3T64k7i6m5p0Q38ztASD8B&X-Amz-Signature=5cb927659080afec7787c6ca399b57617a58c711ce4f5232a56bf13565ffa7d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


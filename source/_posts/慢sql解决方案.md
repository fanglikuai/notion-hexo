---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVZS26XH%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T080105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNmdvE8%2BUY%2FU4Aminu%2BWpdgb%2FOw%2BZ5bVKcwwLZFXSyqAiEAkp59CJGq9qpvaPnqKp4D6Goh0ltMipImsV5UmAw%2BaEAq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDCCQDJWo8jmtsmNrYircA3YEqbUBNQ9VlBM0IFEEO%2BbxK6AdZG%2BOszzNAJPnbQX12bULP2spBp291PFwdu4UsRIopkeTfAnLzQyT4KWTDNeVtx3MhuIpGL72kY2BAVtsfXSdrNjpZnVD0DitrDOCy1MfSlTbi8Nd%2FT5i%2Ba7JMVzKuJtOihrzW1xmjRjb1ANBheCMGQL1HyALqEG0ezuE8GJEHMSo6AxpiCqndq%2Ff3%2FTzrvgQ%2FnH6j3D0zLDC4TBvje6z6XC%2F5BToskA%2BhaCPj6eY8LvQMZyXONibSqXBK7z3AF80tvfmTzyMxjJngYJwyz4MUhzpCx0xYT2kn%2BWZjWBTAHoyn7YLD7Jz7V2xTREyYHntuUcb5KqocJ8RKGeIvSQc2IWPKSosKx0A18KxI11GClPOzE%2BCXAL5j3Mgyo3YB%2F7gPgYBa1%2BqcmexXJeqZGdJrvUZrycdwMh47aE6zpN4%2BX5aPqIyco2c8rHGbnyqpKcoVOZ5bhN4MPPeukou0DGhdduA5jQRak%2F29DgX4JOOcA5I3d2NeQMI03PNWSEEOJUPDGSkG6x5%2BrapldcoFg%2F%2B23orTg7SUA0h4sNVtyRxXmSBi1quCZl7lfkle%2BLgUUJW2TH3opWUAaEW9NqGId2caDuxGv%2F89%2BocMMq1zsYGOqUBfH8NGXmS1TPQ4h%2BfQZesEhOssf17WC4WD8NbxWijhZFVCwwzlpa3l853xCqC12kdjqgqhYHxQ7azhr2Ou8apbMsIYh3XxT%2FZpXQ1t8y36DsMQg4cU9AmQq0SJeUlspEV%2FyPzx92HJem%2B5azQcAP%2BklsrdxRQT7RCGIOjZWzZlIBkzyZeIPhxuj6XwDWMsfX85yp%2FKAY6JZNqot%2BMlxV1gBjQ%2Bupw&X-Amz-Signature=985ad40c72cf397bbccff005fe0e780817900d15f07d3d4946c0f3ec2bb052c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


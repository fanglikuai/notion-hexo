---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VAQYVB4S%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCICMZTYXBiBMZG10WhVRz7ouDZuxjOIEMxAcdHCVXUApNAiA1RSCZ6vDOzr%2BuOLb6eZHWHpc6oQS6R8I1TpnBYtUEZiqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLZNCTttdD31pJoo%2FKtwDxbioRNMEoAcqin8sZ72W%2BoPI%2B7ZgQZeNOVifNnEEpWtl%2FDi%2B7%2FIGvxL0oQpMwt4bfPp8Xjex%2BfLO%2BCvAHJG4Anu14GYqEzMKwi3DhxEcC8BrV2iivr0P%2BTpYr3JqZkjHccCLDPHpHPEj%2Bf38tM7ldCbENTjQww85ksL%2Bqi1h5N5zpspJ0yqFfEvhOrs1dfG9M%2BMnyEQomgRqFoEavMKpGXOk6uVQPiI5vzxehOGar9WZhzSoEIBxhU9xMMlYCBRV%2BnTkIbAt6r3JkaGoAba%2FO92PZzxSHI%2FXceXMaLM5bJfADiWNFt5XJfQgDDMhbgVyCEtqJaD%2BDCDRzqN7%2B6ZkGeG34%2BLQ%2BghlJYr6mUgvkjtxtVHhLveB4rGeHQavsnOy555OkuzGmrTPLg7H5HXsSQJ9x1WAR0oFMttlJ3g24ozFV9k1sCZ%2BjBBCxdnbVr4jeCTc51QDoDwB9BtNLY%2FBcPSQ2swYD%2FuGMozcr7h5DowGUt2kPgrYfBXCTq%2FE68hYujM3bsKllO3L6hFxKfNjBrBLyIvMoK0AuDdjiDgSFMMF8UlHt2BxIWxQ%2FKCb9%2F34mtML38hGr6rYljvuufoAjM0SVRSeIZn%2B0bBmNQrxbu%2BHd9gKTPK3Nox70hYwj9qZxwY6pgHCUzURt4tiWHwcHzn23zQvgQRTkXYbsug7YxgMRxzHqsmIFW5y1Fk6C2n6EINkT%2BfebPI7phsQqsnjknC9IOKfZH3RowYJzfJ8nm4nI2fV5Av7nd0WMRIlrm6qDRqaMEoQuL04A1c1fAPmPvP979M8ZA947s5pXicNnDqsDTrtJ2Ic%2B6crWuwySyl%2BUWmY25FXxV3BAByklQRP%2BFjC3sa78iB8ddKK&X-Amz-Signature=5fea6480b4517734f7e2993ed68b57d95ba50e8f436230cdb0d186b1642a8e48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


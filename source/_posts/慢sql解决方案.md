---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3YCIV4%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNDJBrtmWB25SrvmM0rV8sv2R8%2Bn51J%2FkFEnchHVie9QIhAIqjZQvI4xrXVtjXIkngwN7Gdfx9OurQCXvSmZCaD1A0Kv8DCEoQABoMNjM3NDIzMTgzODA1IgzBL46mgqbaO5SIvyoq3AOIK%2FjF0OhHzjrlYPOJC9ebNrBbgQBtJWmaqg7Z%2FbbnDRS8FtDk2pIvBgmt0FcWzSOn266FgR9m7WVmHQeBmK3c78rFNPY%2FiMzFAvTVCabHDDpnYUJ5cZBUI2ANp2a%2BTg5B9UkYtLLKC7rv60HbMOWo3Veq3VEEW63Z7DFGnJm2G8EvpRgm3TehWiaI6GWID41XadZFFcEL1oJvGIBlLaMalp6RkWpTBlpVNWH%2FDV4WTwEEHjr%2Bz5JQaYzNXaSClDM0TMiyNMlN3OLE2ha9GrSspCp5pPKZoiKzQ3JHdW6McYFacksyFx5Tmbo068PQRird3trUIh3dPoQ%2FHIvHBp9xSHWBNQuULyPaWhuB0MrpiLxRuiWaUu56%2FDdzmAnPsU1Vk93rjBt0GRFyAZY%2BdsdNwxANj2oI%2FsjtOJxjDuDWbb6vWy9uWyMZziW5yabXowJfODBlC3mFnyTMBTkSRP1iODWGunbmma7i2Y7pylCo0n%2B46%2B0TOGmPBXXH%2F6yr4VsCOA7Os%2BgasnZnISMpFD46AfJvJAnneRtRmJSfM19AJc6hk45R8YYBG%2FTLZzmKHI9I5olo%2FbvC3GRrS%2FJ3LY48pqPBc9uwYVIrgWiEk8A0A3P%2BFkIuDyRW1%2FmW6jC3w9bIBjqkAUltBoLtxwZcw6RR%2BcoXAbaHfmlaQqm6v%2BQ8sglAuGuVcY8IKfc1AtoHuRtnQM7DmtK7DlLvz%2BAj6heQCa1P9RMOu8QL9KHI9ly6G4eGx0uf7Y5LIG4s1D6z%2Bjde%2BUQcQo2lAE5r4%2Flu5gxaUdbMs1Ju%2FoHa3j%2F3fUKvXOYfmrMfunO%2BqJL%2BqDkJiDrYf5HIdjiUrmKw%2FNAD9ljE1NKzIFaRaM3H&X-Amz-Signature=c6c2a26bdc8d04a564d640e99a00bd4391ed21b14446d628345ce8aed2288a5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3ZTOAG2%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIHC2y1X11gp46P0%2BiS0mxgqHLthyi5mjMq%2FpyEqjxW6wAiEAolhALmpbJEoC8R8nXUTa7wD7HaW6MBwuwidHjEcItZ4q%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDFxPqo7zYUliNUcSgyrcA0ae0VCbNuSABK4J%2FbPXY0anGmWKS%2FogGoJeahoFKP6mjM%2Fr46mdsQJD7gd3kah4UwyhGasoY33dQOpkYqLzttnBv1dYTolHuvhk%2Bnj1IbZXkwOrgSwAtS1ixnHKlDv1FgJWu%2FV%2B%2FS95y%2FvWI5wz8EpAF6SkikDWZxV7GvX5beDP7Mt4fsbCspGK5dGBf%2FoPTip0UzGzkv0vmlFQtrD6%2BOZeCIKU3jUqnGswPNoP57Vxx1e00D%2BF9CVSEZwBM7CPCPMIW5uVq80i1KCrSBQTVZfmxdJcRjinM9AGOXIKJxtZWLRK7Diq71oXfx9aVdtPcazE0RrYSi6rBKIrk37wovw7%2FtQ%2FtDUKuvERHSnhAsXg2TGJKmBud37gU%2FMpx%2B2apVMHW4MvjNwhvQC%2Ftg95zXuxCVLqUIXrjNLcPf7W5fDsSW7ocjMAdBWpg%2BaA5uZyrXzOrhG9t0gz%2B%2F6DsTt16QS0oMwjlykXGEbaO1epW0WAbrZgC%2FKRPiqCqKTGpJa%2FBMjWg0YqZQ9jpuR83ewTVYNxI%2BBdIklQph9tCvHfhK%2Bp%2FCqAZ8wpDxwOm5TdaBtwb2GAc5EYdRscwvA%2FCPVplDkPuQ0rLvNvMA9KrZjKKLyJW7ODvvz2afhpdoMvMKCJjskGOqUBbd9pBzkYoaXWnk%2BRZyI3Di%2FctG9kT3T5%2BAB%2FgffSSx9QNlNum36CbKDW4GPwaARIwNFc9CdWb6XgASljo4bUoaKdYF2EuTl4UwfiZUL2BRRuh7DtZBaiO6xKT63wSWwjPa23DWIfmRIuxD6oK1WqhbZZiHCxgnYZaAhW8nGRSq9rWTDUQBYb2sxZ%2FPT%2FpUgbQRQN%2BABVgqIcED4Q8DaLSMaFgIU1&X-Amz-Signature=772faeb8286ba0a9d23105c2b5f2ff727abdac0ad8b67560f900a15e805b57b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


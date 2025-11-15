---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABEJYG7%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICaS6Bxq9Hs5LFCVBMz6SMGOhDbijVmxCQrWUF%2FmE8ssAiBelNKqbAWXmETSPGM9Q8KnX0fiL0vQakwc1eyYqbkGCCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMQBPeyy9FKnwnP7%2BHKtwD8hj42BiTATMwAckDWuztUhFp4RVrkEErR%2BQ8avd1ojjCOMrJDQAGtJiCEhQRjargRjD3RP5U0kiRSGT25Hx87V89MkwR017Ra02A0CSCh5h5CgeUcTVCpANlI2evE%2FBYtO6jCreqFbg30XqWD3wWunkFmPBEy8Q%2Bhv1H8NZIJuRAT6LoKJD2cskQdP8Hg2ebGGRETT5S7a%2BKNpHKI00nofA3r1yd%2BiLB%2BkVpkqwXI%2Fn8M6FtDum9F5YnmwwCoZUQnRRyE%2BoudwWPWz7rzinIwDfOD3swWxig%2BpKBGNg4bLiPaA4KoTjozL06M%2F7ZYx03Ics8MQGYf0s3ppahHYfcC0n6WI8zeJNkbnOi3MyAscg6uRYpCS4GbP8nFI6NTKBXCoG1pt%2Fl8bByfMdKHEkA%2BdymCYYHHQGvUm683jRZpnCxlRUzlIAB80AKO200Fe1cuLjMToa742mG7pMAZrEzPj09gpOZCzBCWb7Cz%2FLrxFIgUrsfiCbX02cgIQqbAER66O3dog10IXR9%2Buep9l0YkKXeCgrvjNXjBpS%2F5dJEP%2BJfjepz0AZWSzClC4que1sFisAotcEBc5A%2FRl0cLeMcHuRVnnAMsU3um%2FtI%2B776UNw3gA3Ze8iNp3%2B6FBwwmYPhyAY6pgGf72n22xfZvH3xn0L9Hapyk2rsXNM2LNdwCdNdNg2MBvzC6XhuaDS6S7eVoqwyjBuu5KvY2rN4PtlB6UTMlBDuLefkZZSOl%2BuOorIyw8Cw9jD3GzaVPIMmpZJa605tiK5E7jlgZV6HpbgKUg4TGEDyqBSOdV9%2Byl772ESqyQxnnx5dYx92QNoShDANftPHVM8R%2BMyA19vyXewjmNr9TG6P%2BfJqKlYB&X-Amz-Signature=e244ce8855c39417eeb4ece0b89d01b159a19b1054a15ff48f86c5d956e66168&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


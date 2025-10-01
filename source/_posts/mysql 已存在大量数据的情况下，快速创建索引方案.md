---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFBN7DW7%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCeqVSCveuOWltv%2BY5IHAMHtp5aI%2BlxN%2FGmMotksDvMowIhAM%2B09GcyRNwrxcmTJ0Jk%2F%2FUJH5n6FBmuGxqICm2GSpHuKv8DCBMQABoMNjM3NDIzMTgzODA1Igz8Gzt5MbJifcLLCeoq3AOxApX7AvOamiZi%2Fuk1yAND4AiyUNiZw5I%2B%2FNUOSDBlyhpWs1ibomfUNeonos%2Bz4SZz%2B7McX8AhLcqTBINWxWKlGyBY7opEPnR%2BwEK3m01l%2BAyEoImydlno77T1WG%2BAPZModJ%2BKyc4dHJqaHUbswAZutex6FWXYIYOGeRDfKUZv%2BV2gaTjoZ083xemVmWKEfsuDYhwBcmA%2BAjcrsvNFrS7trBlVWP%2BsuTBx0TZNPtNnNhxcCZYKXjCVt3ZNi4nyQ%2BSLocqu7%2FZMe%2Fp1fBRGpTCZktM9YshyUiE4S5U4%2ByRyj%2FU4yTo12MQPDlFgMYdLoEWRT2W3ZEReFJR3EdveLvPy3gdFc8A%2FMo%2Bg%2FAiiaG0HE7PIZ4wOoWDwYT2vvuHhab0IWaewNUJoClMFPkBIg0PX3WysIz98P9DgM%2B5aR0LutirjZKqJ4v1T94ziAd1vcPyPHvLSxGC6oFC2S79QtSG9Nlw5THHiS9p3uV7kseqLyhyWzf5smooAKpdOJqW0MENi%2FzLna5wXss9%2B0N8KLhrw8eD3JJY%2BHDCYZLQWA88kP2YtKQfZR3XVPhrDvAGgeZJqZOasVvqR2wJlzKWZXnM3KTRuqJcRh6KRHg%2FYLSBD7ZRVaOr%2FdfFWPtVtWTCc7vPGBjqkAZeMA%2BweX99WfC4HlcV4KE0zQiXtSDfDrhvnP9aha%2ByFSul%2B%2F6sts0TRnqpPa1%2BtK6sz71bgdwMxYRpk1lOimXOJ1YVkV4SbFWMj%2FdDSafrgz0B5GIy2gcd3hTVjEcNlloe6lSZXscwBdm0jrJnqb1RzLO3%2BjcDCVj%2B%2FgAvKlbULr9wv6DUf1PdmEYPShrClyM1uroQ5%2Fa0Jmg1gcjrYGha%2F5ugO&X-Amz-Signature=7fa93b4defcca547965e73b03ff85f3839cb48f4c773d5bb8505318c52aedfd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


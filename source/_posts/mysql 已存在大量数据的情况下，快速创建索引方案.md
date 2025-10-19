---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRWGXZ2X%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQD2QQi2XSipIlqPOV4rKgg%2F6uQDpkpkC67nitsiVYj%2B4AIgbFOdTgxFJJpPsAhegDfRJO%2F1liRuoTo7682WQiFVvrYqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOsrp303ImD2VLnnoSrcA0qe7y5GBjSzwODo7RhaB0%2BJ%2BQlSgMPObk5Yjhcqm8I2mUFVPqyFE5E9NTqOEfVWxF1WUDmuH%2FTyx3Y3uilBURzZxsSN30hekJXm6hY6LaAhBR78bIM3juymxHqrmE6%2FkVZ06%2FtUPgc285fnDvc9I7xBUBBqUo7FYVz%2FH6DgZ58JgpZVVK6JFUzmR%2FUrL3SVK0BzaJOlaqCr1LCA3Cc%2FF%2FX46yU%2By2sKGasrgANhnW0Og59kB5P%2Fdlcb7Rc77f3NmXdWbhWfAzzeHXfcZZPzCIwS5aCQ9F5D2vCGHghPpCcn%2FqnNdyHaiwqD%2FaqEHQeQIv4kU8D8HA4wZFnTRplHDmEEvdR9RmfO84BBIITOrUM1bDfW%2FJsjmpJThv5wqxsbi%2BnABhkMLIaV3k4AKrey9R9i%2BwbajM%2FMnIJ%2BNd7JSguPMNYIelRZXg94mrz9mC2%2FwV7wBoLQq6RMFdHLNuJ4hNK6vkK5PaFM%2BdDzqtcNPEiNWsSUhuxQLP%2Bk%2FQoC5VXy5Jsv%2FRnxKSdFn8ZxGq%2Bri%2BfJeIWXQYJtGMwCLdtFoGJMBlVLhnl3WgsG4eAnzZh8QNxtF4kHfEAm97dfg8UvPhTq2c57f1gNdTVFfJHO6DSZSSULSug5uRY7%2FKhEMPeN0scGOqUB2ISPLraW00SWbYx%2BGf%2B3a%2BszyhLaU492BA0aQpz%2Fq3eHgfjnxBWmIwlY2SpbGz6BBV2LuWsCP8ihAH73ypW%2FrbuKFMoxzPlEHP%2F0GLVhQ1lSxh03mSErV%2FrC1UgtHKwT29ZcKggh53w0N%2BuRqdwxn92hY1rJbdexHvxNO3UDrcv%2BPHGQsAFcn1ebc5kANABh9SHwGMPwQXAy0jAI3pCVr707UZgQ&X-Amz-Signature=047ce45fd9620832a502e5c52ddd87e25eb3614e79db15861f1f48092f7caef7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


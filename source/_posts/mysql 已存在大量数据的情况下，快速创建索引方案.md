---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIOEXYXD%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIGo5GcOlvcpycU0bwpmqmjFLID7GoVf4XZUVKOfKjepnAiA362QYBPZ7VDF1dmGn7O69wE4CzT%2FLTDd37XTE0%2FbgUyqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGx2i3V74isyz%2FwR%2BKtwDziW9samd88eJPLlR%2FqSvmXoUXlsKhVbu6gcQvM8gURoDYxeE%2BJLWGa15qnKiOLy%2FroYKj9WZh1rH6c%2BqjqzdTZoukKmFQ0VofwOh1omWdO3xbXGIfy%2FwffXWcD8%2BYqyT8oMYfaaZSTtO%2B8p8GGPZgYyWBuhjHYCjjMFlN0qjmDtyJvXbU9ru7hn5Xc%2FJ1bITxuEXBZcQTOm48Z77RSQnZT2wTFxKCrp3r%2F9QD0RhJqIT%2Bfd5oHRmLd3wJ%2BEEf6b6yjJ2orwLjRB0j4%2FD6FXlklluLCq%2FJLeCFZf%2F7ZipqIOgtFWrkDiuoKoAfKYEu4FNkWGvz7xNDQ2qgz5ZXFTJ52a5bJxMr6%2FpHN8PglSSDSZeoxY7IVERuI74DOmBBQEKHMshk4pZEUg0cul%2BZJp4gEwszkPU6UHR4%2BXo6F5DpGYEGoXrATOhe9sTiuaa%2BOS3pfVEIfPM34tSGvYrF6FBSQt1zDd%2BIm8zw3En6Q243S22Tz%2FT6r8lHQIHtTpP8ZFESdJGjxpUrxsBjGmlgMZ54HPEOp2JGZQgaSo17rFWasrbDaO0%2BJnEbwFntKBjMiD02yHOXuWnOl%2F6K%2FMC9wzrY0bf%2BU%2FFiqovq5Z4Nu3iwuZ8iBujVW1mqUHFg5Uw3YCbxwY6pgHEOYkju01baLXMC3toEhX5VushjcSz4sccEKpJqT3qor%2FJyfe6jdvAqlRv7e6dcal1HvM3PPg9WMFQ4IWxNARj2VFggAXKpDsKxOn%2BU2%2BEsX0bv5wt%2BReZBYtfWCI2lG%2FacHaBho8iuQpxGcfAtcsjKUUfmDIDQt0nRRmSodBgUxygVPMkVMhB2InZHngD4nyAPz6oKbFt0MTJ7kKhUzDvYcAqM8ZH&X-Amz-Signature=b2b4833e53895fc30b2f938702c66e845450dfff2e461071fdcfad6d55ec0bc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


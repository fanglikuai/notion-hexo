---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4TZBD6O%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T100107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCIEm7sFcGrzcMmYI5yS9gfIbAp8c3ElAA%2BgRWtEy5DhJLAiAidq42cHjUi7PJCDxb57fe5pI%2FW9p9YrToMdgiJXHPHCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMJDy4oybPzFDRgqoMKtwD1Oln34UqBuZeCQ3OPFDKkyJHOrfVKMalhze8ixoJEX5v2tL1M3S1eYW04XNBjHvRkehWIThX2mHFIqi7lX9Adb3KmItoCcuodWS%2FPyOGxIqftA%2FtPC7k6FtI%2FeyyIKRJ2M1Y4Dh4szxR4ODK4b%2FGrpME7iYPG9slvYYz5jPYcC01WV0lA4E6N2O98%2FUdOoWBlG5JyjlRywUWJROpIxL2qyNVc0zUS7QDpGQUpjFaJtwrYecA0M3r%2FkKwaF1u%2Fln5wHogNU2Vb6OwHJVTEwNnMMso8KvBaRg82YsyMit11g9hZ2C25h%2FFObJjsyrHQyEztTOMcXtoUakGTjWZCWSvx7fIN4fy4Oh5pWeGZiNnVI1QmH3zFe6cNRZqs4DCbOYLrNtghvBauKSbli3JDJd1bAucqLgwrZetI6nI3l8WBSBXHaF%2B5P5FpjptnLMRKieBMkpwD8EPytXLL4%2FFWoNy28Py7XCWQsHV2P4FLCletC%2BCFSuoZa2tvKf9UdPIGdamwiDBrRtJjg4Uxh1kRKZbljt9f1Gok%2FPdokFxZ0c5hq5Ifg80lGW7OXsNxQBNQKSx7HllOMxmui3eYjCxWBdcyT%2BHBDbIIdcfLVOe6smP%2F9BZeCGiGr8Oe3fmDCgwgZPdxwY6pgGDh7YQqlOs8oaOMoz9KE4ods8BgwknuK24HMh0TL66HIGSZLaCNh%2BwZ3tQkrEeHxqCnIvDGrgM8wT0sJ%2B50JZ9weWBVh9JfQC5IKe2KxWwWB9YQuSBaQpBpwfZ59Rl0n%2F44or6jzm77MwOTXfB1OCXGwudJqn%2FvrJctqMfpHMbJQ9oPzQaOmbpeIxhVLx1toMdvgBt7w3%2FiCmRJEIm4eqskCV%2BiueY&X-Amz-Signature=9805f0ebc5959b4271a5a66cf6ff96977877da3eef5a5352f87146ec105b75d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


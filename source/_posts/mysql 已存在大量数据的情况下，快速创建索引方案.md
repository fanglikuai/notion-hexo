---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBH7EYYK%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIAdBhKHJoOMsZVRiiV8hg%2Fq12ML9RkgYUOsJNtamnYw4AiEAj2rrkZieE3Wm7Hjl3GQqT6AJfhp5Klk4lQUclIW2F60q%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDDXK0iotR8ZxDjzzuSrcA%2Fd4ri%2BXBikK%2FkF22nHUdkTa77a6d2mhMeuwVNGWxB7bnktm66JGMGJ0lFVW%2F7yku55ct5LIZuYJWQSWt6ysgXQIJ3kmFzu1VSIfAEh8oPvtmOWZP0e5AcloRM7efUWd9JAv4OokLl5be%2FnreGXs6fnjvguxS7beipjkv4snEN2V27f0QHGTv%2FF%2B0mqKa16K79BenboQmqcX%2F56AU3VwbDtdlmCjjXWn2eyr8iZAkhWMIe5iJeGaJ%2F9AL5KOv8o8RY%2FKkwJQs0dAtiYAN2d0PZ%2FTCHXNlmlnSF6SCRrvWOMRAc61xkSS%2BfG3ozN%2BGbWgEWAvfXR56aMuipdr%2BjXqapGGWGu9o0r5SEvQw6UblzYBh%2Fd%2FV30OJhhBLLs2Z1pdl3AIifZI92hQh0zOK90qWKEXAYTjR%2BCTsexaf4M5NIn%2B%2FPabcajpsDkZWzPhJEQngZPqCCYSQdYwmvRKthSRIvMkbHUVAcj3fiVpAT0Q3u6IkUdzcsvtI8iv%2FjSXOspXj64czl12LX1CHaxWiFVOb0L5wu6640i3kD%2FfTB8sy58uhLQkLpmzJDom253svOKA9NCfnp9GvM4gHsOrOUxherX8UqF0EmFBJYcjs3WbffKEtKVuluQ4bpzjTB2XMPj%2F9MYGOqUBdfe58p70IqMMbyMgtTFtBpjYyP3p4r43Lu4aMQKzV67tBTy2m%2BkQQhy1QbJkFYlQAYpYiL19EkY%2FweD%2BByuq6kiwK9lcDgliHHUVlMF%2BNWGWxIYciLyC0jmy0UUHO4wpT%2BmxTklVrYvc1%2FgXWgTwJeZVVm%2BfAxw2ojsghk1S69BVMyXw7fEHYTOpuT8GtuY8MtN9y%2FdAHuOe9Jbvx9VOWS%2BY3MFf&X-Amz-Signature=441d1efd99d8cae3e8285cdcd0c2a43db9acddab8cb7515c90d884ff3e5d03de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUIHPF57%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIQDSKlBMpJ37wWinKhNsl22d2VPyRL4Od89kFiwLhaq%2FngIga%2FHMbqXC9C5YLlOBNvdkiFFCGPwMz%2BwEw2tFD2iFHHcqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO63TmJx3ARNRL4C4SrcA1ZdBJOaZ8BN1OYb%2BBtPI1zkEr%2FtCVIOGzGU%2B7DdlMu2CZ0%2BEobJLAGOpw6WB7lFDS3WnORxwY3EngIOen3GhZ2%2FenRelIyEOHoUn%2BuyUh88yuOXKB%2BcwSFBhSOfYxwQqVVT%2BEOAM9S3%2FadHrQNGAxyVZObtjjMLpOkDJvNpEp0l54IIX7jZYfSX0TTeKz7DqGSt0xeiHMeQahn2LQQt601dC7qE%2FfIeEamhB0q7cGPGsScLPRcWwZ%2BS%2ByHJQhT0sItlY4gsp%2B5aJgrB9x6j3IIKA3KTjXHLhwkafkSVW6ZX9YhAJu6nQ1cF9IUPKfJ5Lu62W7HTDlh7fC7B9fxI9u6X6YKBj8t6pObUVhfjEMuE0BwQoj7xeKi0yIDsmz9boZu7mduPFsCbgedW%2FXMj4SYDmJZVSZKeuA3e0zmYCzUVpc7zBdz8yySvSxMozV1oOP3oMhDVU2cJ5%2FiFAOipV%2FbyNKReS7G4LO7usBOd4zNIeTRL7ag%2BesJPxJDnema7pyBwAR5AmPwx4gnWBhQPi1b3Rl98DsbhiVtCodeHnFVMSRShQpUmXFYn8OdZWcwl%2FrLuaIP2KeBdanTuhgUTHrPY99Rb1YhfIg39Cb3GvVtIEZeUEhhGMoH2KvMzMKDmo8cGOqUBAFqeuh5XMWZs52w62e2TMDJmkcOnPh%2FGEAnagDmisGhbV8xHZvWlc8qAI8Nuxy5ElLvL7s5FQK2b%2B%2FtDLzJCjbJIi6Fmb061QG0jm53MRyjxA2fyX%2Bh9y8twSVf84QlVOSKPaPegsaPmiEefwbg7j2f7DzuIkgU8F1zyx7Y33aV0v0Hg0gWj8ljYZJIq1f0LRMYLosM7KOaX7nDe8wDNsq1cTDJk&X-Amz-Signature=24816cb28a13f5c4ddd6dbfd5f0de4b2eebbf31b18b3344ee7743dad5805f644&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


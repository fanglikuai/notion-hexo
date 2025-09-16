---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DJ2JLY7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCWOvtfJ%2BxuREM2h0i4nQWGsBBkYlYaD0no2JvDJMM4wQIhAOrzriEiFH7pix%2B7f%2BC6o7kGox24pjhssvJT2uF0SXD%2BKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwHIvkf%2BuX7xjo0vEq3ANMUfJFkRK3fWdWCp1QgQv6tfCUWXa19nxmlwYWQuX6zKeMn%2FqcblnZ%2BX87ZP8cazjo1lazLpTwzJVbPigpcYHvF49yQ%2BKSqht%2FXz6BwDeicgpJZn7u2zyRsyO%2FQEyjc4t8ud1BFQUN0Hww4KacHFFi6CauBQfTvD%2FxW2suN4XlNKe41eDz83QujotfGL6Hy7dXvUVaCnISmrjULBy1rI%2FihXlxX4g7x2PZ9USHubHG7Jzji0vJOVXtmMm5S%2FjswtvA6eJ%2FxZ%2B1cDWZ8c3H9b6vBHpFqa9gdiYLGM3GKI%2BVgCbTpX00y5NJYN65oN3Blt%2BFHUhS8sRK%2BbegXFZlHr7pjlm7syBEO05AOomk%2B59QFjR%2BJ43FsVMOIG2zo%2FTh%2BJupqwb9LP3%2B%2BxiYZpX7C4fLjGGtLJqYN0GYlpvYWRDKsZeu4vdKfbhLv4%2FzgOFLPdcbDnl0sCb6ok1J58E%2B%2BcLxswIt9%2B5tJqaDFnN%2BTII7OWjq%2BN%2BYyfAna9UTEF4kmybWuGtIIQoys%2F02dZMknkgB3LI25RXxpVQesSSXQkl7ZSqxHEHSvjtOxivqFfS7kKTvjddVBmz5L62fVT5WK%2BMcdam%2F%2FWjtT1v6PkfIRppzeCwNiGiHQjKyaqCcojDS46bGBjqkAToRxeDloxwO2KVOxBXoJYnHrLi5cm4FSNHA8FWUNCmGAiRrLgngjIon5RR91y1wtQkIJMi%2FZHccErp7TsHbMdhJc0r%2Bm5AcHVe8N43vHHpt0Y37g80cKwY2ZdmIOtUXmtZF%2FmBWw5tYBPwRk6sBuzQLEqKqdpoew946O%2FOGbRVGjgMm7RssoEGRPOpTM91wbC40ChL3VlcOG06vcExAqSw3k6dN&X-Amz-Signature=27d0c128a204cfb7813c24abde24150217a7dfd2f552887e70fcd5ec8f1c3649&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


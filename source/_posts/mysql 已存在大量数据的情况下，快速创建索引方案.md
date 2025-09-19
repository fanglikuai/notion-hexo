---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMJ6A5T2%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T230049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQC8w6c1iGEyG7X21cF9YBgQqt32iclBUgpyCVmHFWdgkgIhAMaz%2FXG5e4sSORNRMXJxRe5zvP0FuVR2md%2F1yyqzJNbpKogECOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxaj1SScgkkj5qAMYYq3AN%2BniraGo%2F%2BlzQGaW9%2B%2BmT96U3yTJxhklGJ5X261QJUcEzdmbu1FDoewEL92CdJK4yYuP%2BgYBbwGu%2F1lE%2FXWh%2BPHoi1fwL4%2FJdXa7gUVUDMqcV6VZvW1W9N1XDAHPmawCpiXn4A9z09AsbwmuCqyRks175xwG48u11n%2FDfCGqhk2ToDO%2FXE6oIyLTG7RvOm8rGjhRhWenOj0vtK1nqg06BfTvoSLdWdQg7H51rFAzFyyOmunK6873PC0tI5U1JvSuNhpKfstv6ncNHCH5%2BWpsStAM9PWW1wK1c0oAo1pEsgYYEVOIEBqHk2eB3%2FjHEhPHmdXKf7b6mvS3HZZ36oekVabrwPmE8tyl1BPiKI7S7e72bcBKkM3W78%2FGv8EtHJtJL2jQ6fTJzHeNjZs4tuYl0PeyDW4H%2BK62uUvxUUYIm0RuZ8icbVa9EQjZc%2BpfIz8fz%2B6p9jDCILGVLwCMyRgP3oGSqaUmDpui06enIIvoYKbKNINVb%2FauIY8Z3SMbg7aEaxxyGMCIzlPgU1dK%2BDIbm4WzQyfSD1EihhoDhYbYYKAxXBVuyfcV05kP4uWoR%2BZwY7vN4czbWCeMq%2BPC9YGZcHAE8lysi05%2FNy1EmRbEJD51fJKYgWQIgIxfCxPDDNr7fGBjqkAXrf5DmHsk7XoI49WMb8WaXWqECpXxKzFQwZ6MK6xIJncfZcpStdPWIzKLElb9hWtuS75%2BgvNrLvK03RJoS9Gum8J9pGLzzvxdopJYrKb9DwJMntYRg%2BJOJq235hOqVTmTvGfW%2FVBYRV1BnAMk05IbSU2cs3r7h%2BG9KR47pXyfAq4lGnbleCUXSo2fkoZmtV%2B6hGjPl5%2FnKElL7cg5v%2BxT3KXus1&X-Amz-Signature=5468b858ac94b4f87df97b1796269da995437697cdf17437b3e7a94f52ff699f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


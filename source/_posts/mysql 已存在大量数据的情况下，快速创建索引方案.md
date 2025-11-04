---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKGVHRMF%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpKCgVMO%2BLHLKk%2FCJDkysw%2B5CWcrLhLYqSkuDdFsB2FQIhANjo3QfxONvowA6GrbcAVGeIrFd3vOmgKGyyC%2BHhAqzuKv8DCHUQABoMNjM3NDIzMTgzODA1IgzkWjdGuY%2FJOZCocIAq3AN3ZFk%2BnW%2FgrGhhnqaccnvfSlr0HEEQVanY48SL2iujQDgcapYo0baeQc%2BVI2fksKr7ZviSF6jWpcKYFvrH7vtpP7XssIBTXf9yKDlYSxnG%2B1GbtE2SAj2rjouwvnO7A0cHfO0S0ue3qeQ11ZbrRDBN7K5vZEWPYQ8rSZNKISoW6jSJ3fUX3wij5T%2BWfPVYIq0hEWleY8aZBmBbgzdOdf56DvTWTTfzKT7dJnd1XtfGSI2AquMRX1T%2FaIkYOdzP4a2SDX73nN992LFq%2Fm6KzTMhFY5eA5R8LguaPVjgXD7LWaCskbTqxmf4Jd%2BPiSsDW7dqF8wZN7llhlazIyio8K5iMV9Tw8l%2BDHx1dqQovkPyQ0m7gkHmfE0rdceOGzYzuWWDFQfX2MZ2F2lqFUHEokhFuXnWy1we7OEWaD4mn8U8tIwGQ8HkjfnJikYH84eedFEN1bktLx9Cm98bEaEgvUFboRp7TEnjfFe9K78DlLs%2BPJ6QBXHNrc6jv26%2FtaJhnvV5UPP8Z5gBCO4wt%2FsMaqNRfYOCY6quOMYNK7mT%2BIJH4nrYUPuK11ayl3w3pW420CK1Dp78RVT6yupja0mYrWv8bEbgBTi73wfzA3CTJ8Cq2UFU3n2JubLAvgpG6zCe0afIBjqkARaZAvUK3JZN4Ldfa%2BsdwFVUVRlgFTswn0rfUGs0DvLWl5IpRL7I2I9FGfDtol8l6dQaKF3UZk4JIYM%2F7v1ckKGtolLo9Sp2QsvkiKXuzoleye8iHqmpLMWNUxRIqeO1iFgvOInk68Uc731%2BSdtKTEE32DFS4KOkspZOmxXsfFeQIgYEyN%2F9Xe2z1RSVP1Gn1s8gUGp1nL%2FdBV8jNMy1AY4Hsg2R&X-Amz-Signature=60067372ca0d591bc6e8764dbc29c5107e298b676379b5a6f3fcf8a065265318&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


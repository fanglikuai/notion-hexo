---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6I3GURP%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIERwH779b58ErCYvauTKFqW8dSQL0ZmL09gZlfhINrnUAiEAnT%2Bt%2BlaIuaC6czNbRKpAqr2bk15goijsrR%2FBsTdh6jQqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkN8%2FXs37OORz1tlSrcA5dEXsx%2BhmFKx7bX9H3E4ugjAKPCD2IQ96PTkCpIXF53mPIerANPQEaRzizGYgMNh1J72ojJLWf%2F7tYVakcy0BtOU6KSJb5WaZcG9jL%2BD%2FxgYD9W4vu6D22T1yenmlKY7%2FEt4rRatamojO11F9xQ%2B9P9m6JmNND81PBOPtnzV4CJmOLb1LEyl0DLACHN4I49yjtCn%2B4DnPUphyErSh7ulFflJMs7ewWZ0UBY1NlIIFlbdqiXwlkGrtKLzN9WHXtCh8Gp5e5SKbI2qhNFa%2BZaGcuiJG%2FMixFspy0jcTIywhu3RvoMtbUNA1xU9qCb57Fas3CivJoEELx067klc5xXwzIu3Od0FOffGmneul5YwPHu3tFNZyUtIgCwugpNy6mBk27%2FKhw2PBC6uqUCeCxsgyakLsGdcKWdxFz1Gf%2BrzCOCHkXbDBO9oVjtbGmMvUAyUms0ybyWrYQAUCyI4ZbM638FK5m34htjQNk8YIsGTK9o5ZxA2M7paupeLEogW2A5BTIq36cfG7jKkMvxdwrx6kjDznLqoMB44wALiV4ucl5mXj%2BHQnSWIbYK96vbngEi8sUrAqWanmNzcnxslYnTZqj0cj6%2BUXDAXr1Pa%2F%2FjBgHGH5zFcopb28S4qWrjMPzsl8cGOqUBCBf0EJIM%2FyTxmciZLuPEGGmQdCSVEHdGictw8JjUrO4SMDkGBeJMeIhYfDGXSl9JDfhNfws37kZ5v%2FXYwouww%2FwWOsutf7dBWRm%2FPkqpoXxqNe%2B7%2FSJdJdklZV1Y4BouiXHKlwMwLhO4JydFCl%2FEHfUThXFuOhc1CGUkPrm0W82VZSxtu2upGg1okyFVg2F1gEe1No7fcP2wIJ0bBC%2F5YwI6sLjr&X-Amz-Signature=d5e4d4100864d4c684b1920299d47cdb09b6d2169bf1f8b5dd54b49600cb2605&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


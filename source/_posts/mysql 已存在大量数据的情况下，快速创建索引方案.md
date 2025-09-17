---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKYLH5U7%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T130103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIQDKtIE2nDcuxdOI6ogLCAOsBTndUR0dSXO5hWmQNAx8vAIgfqbXYK59RZKcY0UOS%2BnyE267XvgXs5TkQu9xWdOClgwqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW%2B3XwiZOsK70CKOSrcA8H8VD1VibYHSIh419U3IYpq27HfZSPuiqMTnpokWdPTB4yHlXCwsATR8LqvyBu7nk%2BKnFAksfHd3iTdxXO2Igv88zPF7kss97IL47MiJMpIcdwV4j1080DO4r%2BC5dCoy%2BLTlyviRgjJ5%2FolSatUq68tbGxzDixH3FyvDb7Kagvcgw28Unse%2B6aQ4rDZoT%2BxYFWjHcIEQyoAkXORq%2Be7xBk6lrbZMJa1p3seXuIRWBfOr%2BxDrXKxQimhlWG7lmsBH2OtsnK37192r2dNaQ%2Fzevvp9yH1CAfbotg6dt1yibcNXOBk9f9uhPjig9qgiDmfmMscIcRkRxiOWxXug6XBumm052F%2FzMQKZGYXVowJLpaKhj48PIbvNckganbY9y1NT4EBaXgEmxeGguZRL96gw%2BTnjL9s8v6HSJUbJssNzGjE%2FAyPmyuv4yN6MD6Qvgw0TEQ6SVgowC8eZ1VHZklMdVKwElcsnG%2F%2FhPz9JApI8rUbg644gGrgsJKwrTYw9pK0c%2Fjc0v%2Bxc3HXR9crSIgtBIy4MFe5X43zNT9UByyMHnqtbRVHqRmxFsBxUiVINV%2BFzVv2Zq%2FrkrgW8nKVlFpVFnPcyHficgDb4JxOdfPzWpH14pxVjCQIwO2Hur1UMJTVqsYGOqUB0KENV4u7fW3Bswh323xaHte4cTq%2FRJDz19TGHzLt3bN3JpjV2UN1uKYuPNfFUOm60vCq%2F%2Bw777MrUnK%2BqDYjP6v1Ti1Pi%2BU7pWa8P2s44QRRzzyIieeDYEKOVfb8RaWvEb6aQzTX0lTtIdL6THlAQPBCUPnSFTPmjBLWo3LrfpofzGaIFtqCN6p27O2yfgyTd%2BxP4s0J3ui%2BywdZGgqyXbSMG8xJ&X-Amz-Signature=c71e00c4642304e33244866862372d41f8dad4c4eb46155b87022e83c8a0f5b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


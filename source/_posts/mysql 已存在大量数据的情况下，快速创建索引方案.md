---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUC4Q2G%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdAwdk0ur3TAc5S3uatFCVWbeXRijCCb8Ze5fOkzJXGAiBxC798hBCDuScChU%2BudC6IYhQvxbfEG5NrSwybvGTx1Cr%2FAwhzEAAaDDYzNzQyMzE4MzgwNSIMs1RdGeblQ6gll1xjKtwDCqFF8aSD1t7Yy3G4i8G4RQoBA4IiHPdYBHBSwn49Vl2bNoBMGr9mXHYAn0VnziF0BhtEuCXrZyT8A4J3f6if5T394iMkMDLAPN17FsZ4qBDirb%2Bqn%2BbN1NGszkf3CObx8PVXI9SRoyqc1Qdrye1oWpYwrgrAm7DjvntrvdC6Bnp%2Btjme8pYa87IqbbkW2p0aMDYZJ%2FzjNLeh4fRKXi4ogxeZoQ2bDh4p4w66fGdtkrrhtGb4W%2FF1GPQxwgOATCk3I4BwNtbFKo77fQ0iWGDr%2BfwZCOobRUCdIJPLrhjamv0NmiAeqsQ3JlcSLRLO1M1g3mvAvix%2FeBxNFMx4NeRhJI6ff%2Bb7LcG9Xetk7XuVMYQ74AF6menHOluB6dIy1tsaofZsuVbMszD2Q932vyXhj4GqmFLcEfO2TbriLUJcquNsk2NoiLTu0RVZPc7QnMx%2FfZW4qRno6LoGntezSVs5dcvXSd%2FXUNZr43Eh2eyjOvwiIpc8VOPBhCICp4vyM5SmAbFdmW0%2FKXH8Oo%2BC%2Fli%2BlQFNcfoj8lhWqkiDZTVKVH5pZ0v%2F4A9nk2DeUOpUAnekVFftzwuWYYpwdr8%2FOg9O1mL%2Fx6YzXP%2FDQdQZKO3mKB202CfWSyHiu4ahbAsw5pSnyAY6pgEnEcOydm8fYrF9xomqXCh3z7XRtLFyLqE%2FjbgZ%2FTbWXuHI4KgJb0itl5fcPFu6MnLo8AqkkMP9uT6r2zFOoCi3B6yX30vVEeyJmkc8alHJTc4imWPPG8n8ZaaNAI6k5djiPxCQb3EOJ91XejsWpK4N%2BxqUwGGMcXWLJRzO2vbIIhIrPSnZopv7%2Be%2FwlG5mQ930QKhH5lLAeY1sYTdZsvW0saVPi4Ua&X-Amz-Signature=9232567626605efbdc687de1266cf07c22038dbc2ac6a370ed6384bd029d3d04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


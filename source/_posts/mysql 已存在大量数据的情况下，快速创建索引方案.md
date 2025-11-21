---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664V4TSD6W%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJGMEQCIAc97A4moXfQVMfclgf6KGyzh%2BtVuz9aPninsolGGb62AiBkmhu9oyZiBncVHhzmtk0gnli9siFHiVXYWPTAeVhViyr%2FAwgEEAAaDDYzNzQyMzE4MzgwNSIM6GEP5ULsQcX%2B5rFHKtwDsaEdr0Lud84HIpCSU21Yy0SNlHhheW1jGFDmswaJ4dwxx%2Ft6wygkg6wpTNJ2ZtexdyXAvTWYNFG49A1Mdj3WxG7tKpkaAVL7doTXUKMpmWRURsREoYNSwIqJ5Gh%2B0pSgzCbT3%2Bm7IdK0zAY0QCcuxHI5FvWGTyXmbkjqTtSdj60t9JKunxKvrq%2FBJ5Q6IE9XBuEpwG6eLNOaRw2uOfl9U7uc5w46d2UZ%2FHlsJcEQHQmVQfw0Bvr0aoLoXV92Z8g3kYERYFZtSfaY%2BUPPn50v1sI3nMszdtSMQQL3hfMb4n5LhIFhD6KYQ7g9Vw1BZfiUKPofl95Ndt62y1R%2BGKwaYvQUs2oe2cZ%2FlVr5rOy32dvk20i8NgUfT3gMiCQpQIatXyJyzHBWS4iHPdLFi7E4IGrApRtgtafN92uY5ej2%2FKWplguacas9QKGhaNYHvL2JYFWnVQhp36V%2Fzm6PcFrdWP91v2EK2hOm4lRbpJR2A1hyNeCRq40QcT32xPplKUaneaf45yrX9YBrrEqO1h26BNEJ8dnTZbznyQ%2FRJt4nb4V5EErPNNPRuo6%2FOPVSNgFazsXyTOKbmi7n6xChQrlPhBd04o6ZJRfQNe3kfBqyDqSMGXBJS1Lze3DLIw4ww63%2FyAY6pgG36JDeINQbfSOZj4HDRXQudz9UZPdANDTykIO5Zx9T2ZmM%2Fbuic%2BH0riBgkNY%2Fj1Idoz5Y%2FG28XeOnkh6tk073fi2gFh78Q9sIxgl76MFY7wa2psovvZYeUW5qJhqNoyWuiZF6oxe9Yun76S5fc9YuhNshBditWEK8hhDT%2F5dvXR7YR9cWTa7fu3yj9PpG1zyOefuipoEBMLhmwaZX55kFQyldy0K9&X-Amz-Signature=b5b082a5c0c6ea0fe9d0db28c7a8f5957f69a2f2f2574d3399b9717cc2026fcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


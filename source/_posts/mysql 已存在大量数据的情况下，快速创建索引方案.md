---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWKVEDRS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIEa1frbP9qRVmeMYYfAicxaS84W8sEPkrGumLz7NDKy8AiEAz%2FTj7xbPeaFLAeklE8QLLYLldSFp%2BTGfi33%2BYz81tjsq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDAqPbqA21fc1AL90iSrcA85kr3J4P5ncC4AH%2BBMHeQ2qRkktgKY9onmWt5O150tV7u6rpI4QhSOTw0W1YIwllaQesRB4xjB7lJGpFs988v37gthlmkT98Yrus4%2B0sCcs1WNiLyr7U27GQW9h%2Bz9%2FZjQwnntkUhRxMsNMHHeNAUEVfyD85IzsrQUGXZS3aE9vWYNhDNCGL5i8ry2bq54dY8GiFednmM1lPaAGFVDue2w9KQqYMLjC8vFlOICbtRbIvbBD5vHIDA7ZLCNyJ7y01sDBTr6%2B82CQpGN9BClQcMpEnXtzB5buWHyS8KG4U6LxE1bYiqK7%2F98H5wsphVyKgHWHog%2BOMWTaPbCyf61V72lTFvhvjVBAgF%2BYa5MoFUS5%2BE5fbZUvNGOnlDhqJlLRHaKGTGay4EOdRXMGaoAdpv9NLVuHY9DEAFrtc9miPfMp51jR8lLed2MlzwrMYzTFPOHKqsFsfFuCdGgV61Y4cD6MLs3vjWnv2kJKGJqV9Kt9PrMIBe2DwW7Bh2xhVyDScIPYwrNQ7i%2FcQb1thtt3EriorjGqHto6oXa83xhEAVyCg8B%2Fk6ic9FvXNuzNDZZ6X6WKlPKi8Ene22yPsf5YSmnPp9JIrNgkUy79FBLbIZNihtoQYkDYiInVltgqMI2c9MYGOqUBSB4EGXDWDPmZBvkMjKqoyzF4CByQr6aUz2aeqsPCI4S5MM01mQ%2BhYC2XM8VUBnBsL6hKGwdRCHy%2FczR%2BS%2Fk6ZpzHDo9pldv0y2AVOalBBkNN4QRBIdQDHLdrbG0L4ypeioJNYc%2BDMe6lNOXYbobFOe79UL2Edzi%2Bmikk2JhBSQ30VQoEKuY9jCTxMMZVJDUTkIqtKp%2BFzzT%2BntRx8msJyQcJ2ENm&X-Amz-Signature=af7a1474857a728c802e5b32f5fcfd0801ab0ab85869912ca759da6428038397&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


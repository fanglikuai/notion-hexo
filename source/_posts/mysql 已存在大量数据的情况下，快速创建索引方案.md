---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THJZCJO5%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChdtgX4iZHi2PQHRAzLCfOPjEfWjTBSB4rZ0E7pyVS1QIgFs1Y4DWso%2FXZ5qGWRsOOnHW8nyDars7822D3%2B19MPtUq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDJNSmDbWjvgAGJKTWyrcAy6RljDyOSd7XTL0TMBpsVQrPJ0uoQVJa4LjjNe36BPelNVuKk%2BgjhYmvLm3k4rqkENU%2Bqri5j1SgLi%2F6kdp9W2Xi6UgHQn%2F0cmCPDPeJ1QLNvQ3JgcurXJ9lW8lZf4PfOYL5fwikAnlKwh7ksBhoH7u2PX1yQqMbiVONYltVxJoG4sbtwcvmzjDLY7OVjCpr5Lq8wL9zCSzyksF%2BnkVQ6h6uPY745zlvpDk5NGuUM5wmy%2BmEEelNnxppENlRrGXlcQVbST%2BkfM1o%2F8Fbtk0yahGtYaDdO6uFca%2FhDeucrZo36hnzKCTBBtNMSe8d9L2U6zIXJVOGsVMaianBgeQ7Fr63YorrFZLHv8BJwYT4VWR6NnzGCxZI6HvRLS4Ptnm5nObHK0F2wVncF%2BeaUf2efs4zsVZPMtnW3t8JGiFufPq85IpK4DL9bOSSXiij2QR%2FyZMgW9ODWDqKJhnzvw5hFxFUIhUQpGj4b2EWOcemJwgvEyCQ338C%2FkCQ5sajfVlVMjrLhHa%2F8%2B1cVeGZrW7%2BSXtokFvkQ01w%2Fp1greysCbg0J6%2BFVlXO5%2BOKaJRHYFnKiyIzXRgE9Mf3aXjJ0IZCpdL9v79rtTmJjcWZ3KJoT0J%2FZ8rtgXgi5OSGjJvMODN7scGOqUBMj0ObujOWuTDTphYbUpz9Xbju1ByKi0wThG2PT06guArWgBB0bSFPD5EG0QPxgCvqEQ5pVoWnu4htJmmtaH%2FRmehJmCvCYxfZ7od2VBIRtTgLYdxUonFGvreR0PxqjcQfqbS7rWeoLTf8jFOJN8nmL0WEWDmD04hb7tcWp3XzEqdlndBZSmlnBBMRY%2Bhts6tSL14LnxiVVw8mVmpAFRYFK6jh4L4&X-Amz-Signature=91c4f734bf43681728755e5b8c67a9999d3ed689f0dee30ade5ce3a306e2a6cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


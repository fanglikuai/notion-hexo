---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673RAWY5J%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQDWLThJG2R89%2FnxDSGKZQ2EAcHKKp2gvR6AjSbZudJSXwIgPWUrcMrUZR%2B9nFvEbvKg8dCES%2FZ600ZvI%2BXTft2B3loqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNG5Cg2f435P1IoX9CrcAzN4zsLH1A39OM9WK6ou%2BxLOn%2BZ16fHtSBLSQk4MpxWkhveXQdXY7PSNfWxNOFmBjVXFFXOmEEXZ%2Bw83sydkReTYRKLZvS8ThUqXhTfso6b%2FFgNeNxXexaV5nWW7fyj2y21LcjJzVb75sArURSEpu7AirbTPuggu3rCdP%2Fl6IfEOGOVJR4XVIcPF%2By1DkywrSpAqibCEVfUnzKN3PBTwAiD4xXTAwTFclHZQXxa3ouy9%2F9rM2jF9cFSgrccq09xKo2t7OzKHcxktTxU4pqDnnHkLyNFgLldjJyZPoemZXpTvuqtZK6ac%2F1al4VibVgLylLWD8bmAy%2Bc3Ouiby0qzGWPzsROJG0TCfM9V1rPRal56S4luIHiWgZkOseVrYBzHgD5chGHvnz1g6twYfrWVYYGeBwDotLRPTr2TX7aQKb0S2PtLf1hVm65sTdjOzuVhVJWKXcLCbMLQ9o8A3pIsrfBCaG5XA7tZ6W7oH59o8mzkTCyPsdO3ydfkwfoseuGFhlZ1nzphFWkO2mdE3M66m%2FNiF6VuqwwNpQSvbWCCRsJMqML7V0%2B%2Bl6SOeD2n7J4Je0wjDtFS%2BFIHs4GHjhzxJqHMVVPgC2q%2BDHwrWm%2BHO4Cxvgq0enPjNmtKt37gMP%2BCzscGOqUBJxoy%2BzHjPT%2B7pxOJUHS4KITKaZ6pEKy4eCWfT4x8wwYduFmr%2BvXY6FPTvdUnXE2DhtoACaL%2BNPw0Shv%2BXm9VJffp99G8%2FFCclsOcrAZNBXJxC7K4JKw3uPNxaaxvsf842Df7JtExidR%2BkiGF4glPk7lP9p3FsM%2FjM%2Bx%2BwC%2BFA%2B8OFlfyw7KnXw1eEn%2BJ9mTw7QmsfSClDNKFIMlsS%2FFnKWvvYdsm&X-Amz-Signature=cffadd4911897c69f099d7191d111cb2527c04207b85b845d86a68de23dec096&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


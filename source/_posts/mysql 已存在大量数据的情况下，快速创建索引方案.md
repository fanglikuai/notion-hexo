---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKIGQ2GB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEKR%2BZbxdrFhOhFuhC3EQ5B%2FWUWnq1b0TKxWGV%2FeZc2WAiBHkuB0goF0tIFtESyhFzF1YLkcV%2BN3CO9t9zYGB8ZO7Cr%2FAwguEAAaDDYzNzQyMzE4MzgwNSIMKpYhLcS8Cn6aFNA8KtwDrD9Kv1rvgTH%2B2bShWGiCqt%2Ff4IRFlvTQMAH4CNBqEONhlmwKVin2cfVU7PNtjSAFLIbT0nxvQrSNWayfqT91TrgiEiwiDNhmo5e2%2BPFcApBXX7AJ1KTtbaHkiuiWdHNBJIWGzb%2B9Gk2ih1pNrnTZE%2BfIxBrE8CVHnuY2HSh54BtnipLcGcKdQk9GqY91oB%2B%2BDMDp%2FwtuUkCdvy%2Fkh1VJjtIfOZXv3%2F5e75nIo3PYKM6Amcv7bEK9S323spOatZZ%2BxVBsqaxUx20oROB6XUzoOmKdMyujDiE1a7lOr6d8WxsyJW5iNdadFonzvC%2FWPIDFL12PZOlbvK1kySTMHpX7OJjePRHLMNCfVRQ58tASEbfdw0E0%2FdJ4o0%2FT%2F4hOR6qpgv8K9DuWZ5kH013wXTJAuydyuh24T5oA%2Fr%2BKqy09zW%2BDnlZ1jsxNqVK7U5WoECaf8MAp8A2EAe2zezgLlYpNPDOFzVt0nvc0Qf1E5uhEGYyxOlIRMCUFcqoV4gbO01N7ecCHgtIyK0AfngYbKr590ONWWuAJaNG0qIAqyIqaaaBxULfaNEfmptuRF1MgVATX6e9rOvV15QsfX2Ka5gsb%2FlYxGjVyDvtI6lJEPilzEjSWGNqaEPd1%2BvDpmegwj43FxgY6pgE35fUxhqBX%2BzITySTORJR8IGd7SLKdAeHqnP68m4eZRq3ARqXA1BC698Qr9lqKYliF2paQRteuXVSJ231zgygn7JvUwmHxuWp25eRgKtQDy4aM8SHJUotphnCngVkwrasY6zXjGN8zHlhdFCs3SWnULdxmtU1F4hAgiPV7Yu%2BzJz4f45jlcd8%2B0PyQy1e6nxG7VpJpemKFnb69l9NhN18WleFFjprs&X-Amz-Signature=633794189b3cd0325a7c523c9e25dbdf38c15dc1cf9fa793d2a11bc306a905d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


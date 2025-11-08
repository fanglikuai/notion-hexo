---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOLJQ2L%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCID%2BuNytqZkqKjR9WDxU4tyvxy5WfYD0s5ZD3Kj4hFrlKAiEAy2zKAYe46DkIgdK3poT6cwxlBg%2Fxweoi9jy7blKN4SIqiAQI3P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOdj5PbGqoRjJtn6fSrcA0I%2BeVSxCZLGbOkzcr0ec5IClVOoWrwSpqKNw%2BLcMkLw%2B7D3TQL47qzOl8ZMGm3DT1xtjytBBEPBhoInpCIF7IXUudgyT8deChw8xYlN%2BH9anDZXsD6DCpg0Drwc2lghnUtOmMJsN3HHPq5cM4bnK9kQYmvVhcN0VREdS5VMMxDX2foz72BNaGCMhu1bpOBQ6yUvqCh2eqeLCk6R9BOer1HMSui9jOj8zRwBvALbBb10YSCgWy%2FdCfznnPNQlCYQrxAUOVDaVWYMk3%2F9jbMBLv5q%2FAO%2ByQgRaeW8gcvTYPtBB5goUKySTnHbuBf4JbNrgux%2BVKDCYZSsSUQqP6RAASxr%2BMxpD9OzpXnvy0aMoLkxt4QRGGcyYMavpIpiSCTVFVRVfMzRd6LLi53%2Fsi1BW%2FHvym%2FiGA9spILqE1evTOvWTz6wb6Osz9e5USwohfrzdV1LTNjHPwBV4U2NUwDStVPLnAsABOelZzBoxird3YIUwh5dq0y3Fld7oQCHgt5TrMJhTpVH89HrAQYnFKGn5IUBLcPKKDgC9b2NGVRN7JHE0NuCzyV5EnbhSZz6vaEfk%2F0sc4QYIw7SI4lGk%2FQmbd6xHQU%2FzMn00u7Xg9G%2Bda%2BQl4Hu6mZ7ZiKXOk5TMJGavsgGOqUBxbtGNiAkqYI9MSCBPvzm0MsJCQIziFhXxUkTfEiuH%2BpY5NE%2FYHCwMhhVKYsgo8sse%2FOmk53gMn8rfVzdP5gUI0zQcmkjWOUhUGf72lVF0kxD7S1svbXjJsys9mz682VFjgjFvYJgNbvdbaS5IjrjUBBgaDEIMd1gaAZwaN0vHpaNkbybRFGpATSo%2Bhqjlxrw2%2BoC949MYE5prcxhQjTklGFWvUKO&X-Amz-Signature=aa37d7f6691c5a42c8b35fefac0acb88c1fd9ac1cb0677c4a3579952240a8cdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


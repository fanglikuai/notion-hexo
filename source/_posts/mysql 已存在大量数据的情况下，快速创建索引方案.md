---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RPJXX3R%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAmHF4JyVoRle4jvqCEVsKdlbYxGAvjrOJ3E6fkZW9iTAiAYVX9f5G3W6f%2FnzLjlUU9qBo4L65yzLrGOn24yKdOm2iqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FVzJbdHfUAHJE8WIKtwDOuiJd8XleoMEJU0ElIn%2FYtF4vgrA1RMwBD7OouGGcaKnshngOOXptMsKAOLW6FJmo8yUV0I7cSqCOF5ExGpD7HCDRUtyfVqseyrdqAFKKUP3hUi%2BVLFHZmpSG6JmPmYNOOfYAZQ33nKfEiuIN6Fdehx9JhzosdNJSaWAbDzY1MxCN5G8cx9VwKToEQ2wfp0JsmlIH0c5Zp8sCPBb8nqFh4S7N97Q3rXDEoATIbK9W9pBdNINq%2Fw3SjuAjzaIYT374zraP0vbZLo1An2EJHbyplP3smC3gL2JynYYE6vd5ISrfYrC0ImULMtKAEuLJquCaWCh3rCfEvF%2BfOd0PH0C4twPXO9a%2BQK8bnVPYPs7UjyHiDsNDbiy%2Bme1mAzeZME8dLWWG949gt1cIShQ3WkqbFlcv7xio1LP%2Bo%2BW%2Bsy7gbPSo4hfs96bGkY0pP6lDq6mcFkLHX3EdyJNlJFUibE6ZaZ6pamkhiwWEn0PiCCEtfeHV5nB5pXlOYgeQBGiBch8jXx6K8TL9b4sh%2BVtQSSyVnUxQfYluhmVW1%2FjjoTxc7FeFjT0aMxPf9YFuOdqh%2BfKpd9BZPmGcsSISUf7qrODE4kYFQfQc6TJE%2F7WVWgswASI%2FvC6%2Fomd9MrsBa0w6MTxyAY6pgHDYy7cHLbFRb2DB%2FreyLrDiCFi%2BLu36tZvhNFC1jS%2BLkIWl4qKlY5ZyYuOlQquNbqX5U4S0HcyoWpYoUZNfbUAiPO3o0e%2FIUa4mvFzUxqnqX1AIWIkEBi1vYxyMCE9IsGYIAGdvippUZc2NoofpbpnVNjKUbxNGNu1R33d5tYX5jof8tdxOMB78C%2FJ8Bpt%2FdZYhaP%2F6cKB0nawxXKcnEfSP%2B3s5n3B&X-Amz-Signature=5f81488a40b53884289e91eb24fa713d140695822f6a94ff69c69c4dad6c6121&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


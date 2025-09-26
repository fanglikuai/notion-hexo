---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCJDUWUE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIHQA%2BcH%2BsLWT6RPwxCPQsbAO0VpN8hONrDb1lXiWjh51AiEAwZrjR3N1zMYnGCaBC8RdJDNWCghVH5AB24Se5bctbh8qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYaR5SFyKw7%2BWmcpSrcAw%2Bl8%2BuEgmD8ELIR9dTtJgRniiRI0PYgOtgapnts12eYqoVNfD22o2H6HwtWi98SCQtaP566U4sRTY1bMad2gIRpTjeY5KE%2BYv7hUxSKyeuAvB%2BRp%2BvSv4NpzF3Po5eTnORCPjhLJsdrKFEM2T16X1uJASCIASfr6uzVWc5HTlNuy8kH%2BEQsB82%2FryJHN8uOentbWvubJmHHOqIK4q24Y9xAZI9IxJ%2BkK0fXaopagETTKzMs7wEi5AcVOQUDmYnCI%2BjEWNQzRm5pOWYUdwHHSd6s6xSQk5t1ogPrqFbnV9ioBWEwERzsioiUrmaK4yBBICxKJBRbja2n2XbP7d93V%2F13RETRigr74Md%2Bd2q3eMeBIQmn0SeqDwdQAfU68%2FCk5WBjXpCX4lXRBzXeueSHWOgdNu4c%2B8rqnIZ9g%2FZExTkxhSVBpHeLvuPD0MQgZxSE4LiUYfLs80rQq06FWjgIeH7f58NL4%2BGyQwf7WLY%2F39gmyAUjWlCqZ8ShLfxAFToaySpb7rdBSsxnx2wPR7YNLL9ZpsBYVzmSRN3Bry6mgXPJNQjtzzTmod8bWBZfy8c8qpvJMCC3nNAabN4oVOCRWTCtsa47CMWKnXaA%2BMTy5z4DXFclRXSvRbwEw2q0MMyg28YGOqUBnkWlP3IVjXVIKJntC8%2Fb9d3Hrw%2Fu%2BDTnnqib1DACjW53WrmiZQ1GyPYJFzpjp%2FRQGzZd8RjyIBTr2CQT%2BN55hzzu9hAhrnt6wKrzHrp%2BS1e%2FxPSlzRqL9XlsGMXg9ENG%2BnBOe%2F1Usho0Sc6UsE7uFuv2wkGZbpW3v4aLl7FIZ2hkRvCiip3ns%2BzD%2BqS5A%2BA7F4iVlc%2Buza6TJgUTDGwXHwLp4AAS&X-Amz-Signature=8d0ae96d07893dd4a757faecdb1d5aa6dfa7007a7a6bf86e63480deca646dee2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


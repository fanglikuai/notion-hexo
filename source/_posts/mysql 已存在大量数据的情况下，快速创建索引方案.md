---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662O7TMRVB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQD8Yo5hH%2FtL%2Ffuc%2F21HRv0jGUm%2Fuk1XwydeRtZNR9%2FGkAIgNZl%2Bfe65qMcZALZlwfJpQw8DiMgNYQbXjyrZcCH0eokqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLNIlAigwRzllmlaYircA3xa8bP2cW12bBmzrHJC6vz3xabu1l0YEs4vimeogBpFhaaJVDe7qh46wPodXha9vjrjdtSbHCVtLr%2BJJELawkcBlDXoTYhC8yQyPgoa4STObhcKtpwNrPeauSJlayYGPTFHhomK2WOGIjCTyS9KPAHyiqx8%2Bhs53qVXpbCYoflUlS3F3gTZ6F0I6EX%2Ff7CcU0no44iLrcbKWvw0g4q0UtxU%2BbZXP9F9qxDsTU7L%2F%2BbFn2Gv%2FkBBpEh5w6MIleqRgP2g5hC9OJkXbIDtSb6DQaUstB9Ze6ooJ%2B6%2F49nNQFTFSiyC6ZNa21%2BaBvnWtUEOYvcYKC14WKR1GjVmWBje9KOf2M8mAeMulwMXaFX7LrClbJVfSdz0X%2FiVaTzPmU45XIyv1XcSad%2BxVrya%2BS7qwJbkkRiIHLDBIp8uRC651GVAxywQDakNm%2F0cawBQKZ%2BpIxA1yu%2B7AN7beO%2F%2FRyAnGf7RsAyS7r9DcnsdU%2B%2BQb%2BWq3b0jXK%2F4olzUegw%2B8zko4AYommtZ3ZEZv%2Fa2D7hMJT%2F2nifZBzWAP1PfBCGatXavpeNWruEm8MA8RepARL4fK6qPAGRF%2BIp0YVfYmwjPZJjKcNCowzGUWKmyLCnyFK%2F3L38imgds3ePbOylgMK7oh8gGOqUBJrm1F2ME%2Bd9v9%2B4QIk%2BF3POTziMpyU1JkJ33GisfdF6x140gmRVVWXpeZQbvMKvCE6qneXekWy%2Fb39HEIwmjW0Ndxw1RB4VKESE09bIAZSD5wkdrtb%2BOpENohZJybe3%2FWl4%2FJPIFLDOW29s%2BDT%2FNckGk%2Fp7rPjrT9nvrsRzQ%2FCEGerC03A3ksas4otFHUKJC9f%2FnRYczmrLT3YLCGtxnNYc9Va8M&X-Amz-Signature=bf0e354ffe4a2f5764b2ae5e040625cc0e1a9215b72131870524c184d033f738&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


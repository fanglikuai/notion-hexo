---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIFQCEQL%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIQDAyKWJ84JBJRdAqdtsgF2oxx1BB5HIIAuSoLXuTDvFcAIgffjbjbfADQI7sV0WXCRWystKewRE09lMaWSd%2F%2F0Y248qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFOESXv3sbEmtbWZkyrcA6DUVfVE9TGd0l%2BD3ZLXBGISiI2az0DlQnsPzNyk36v%2FBAdR%2FXyKSnqovcCVrg6cA2nAtKqk%2BEKtZ8lKbJVIUee7HsmH6QoyHY7NYt88pvy%2B%2FIFvGtENAlDBydXbWrt628QK4OnwjbQhnWgK5BX1tSOyDcwEgeQS5j2usNAevJEyIx4mHFlLDaQHmRUzaT9hTm1i0M9SjNQnV6ehCIfcDO4ELrnzecgttwPxq4JI34hK8iweBC7SQRhCnibpVU20ypLD8oJ2gXIzRMZd8nKNV7rbVdE8HXMXJ7mn%2BKSxgLezHdDFLxWj7n%2BIjXNtKdW8%2Bql7EXbcB8frH8cRmua8IzYBOnzzW2DfwCrudAG17PJBsHO58YFymsx4Dg0fazifhcHsrlpztArm3sSLpbopgPyoKLmuYbF8iLoAtC4KIKn79gB%2BOnWJAkEd040Hx4cZ%2B40MoSN9g9q02gRg%2FhDgmRRR7LZ9hrmI6wym%2B%2BhgHmcbfz44DqHZUrmxSvqX43Piq4NndUDliezBMsDpfW%2Fy4EzzqZfN7L54jTezfS%2BJ11ii0bKs%2FoT4CiqyiViUfanByLdgpNY1DKN0Hqa9X0C%2FiAUFFtAf6v2kioyFXsH4Wx6igX69s8TYMGaTgtyPMJjKkMgGOqUBAYrLC81yhwttkWk68vZXMo1M3RnF99Airupsr6fkP8RmvCLApk0%2Ba356dLM0HU%2FhM%2BwJ79LrrUhrtVM1d8UZvW5guKAgMt7CMDCh0QW9YJLzdX9R8nYnGBOyjYU0pud%2BsSlXxMGRlugEEX1HRpnem7xMoph63w%2FW1t6LguswsSbTttj9gseqLcJVbseMBZuGKk58uRwTPaH7oU%2F5oidvF58ch29V&X-Amz-Signature=a897c6bb5339a7fefe5fc9d5d5e0f9aebde82994b0577378b6a17f926962bf83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


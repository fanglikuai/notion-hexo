---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PAFPKIZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFXh55A4nlPYVtmZW14iqNcIqE10Vcoqiox%2F0Vy6HanaAiEA7zxFGd6FOK2Q6bTsnq%2FpWE3lBs0vN04wO0C0C9pM7eYq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDMrE5NvOUKYgWuSDoSrcAx%2BfuNTk13buDrCbgiQC8jTiqSv1LK6DowCDivSqbPXqQyYoP8LV2QB5rclOPU2581qqkWi4b%2BNTcU8qpD%2BFHLdJR8UOCzW8bVzoQ0ag1RfwrEi7ol%2F1c0EhgsMWWLUndiUbOthbDwlVd5DUGL1GLs%2B9lP7Y0YhDpEakam1oq%2FdxsJRaMYLqQppXVXdqO6ibNQE23OvAQOlUCtlUeVFHfl6dhq4Vj1VDLX57%2BAZPJGzjUYkD3w6SJqFYWWti5W1ngzdf2Lc0qnOETFUtkbaOLzS0HIWBtRQwNF8r0DY4O1QccGTsksv8BF%2B5urpniDLxAxHnuelMyRNJ37qorsnhSjwwIANpZxfkO6U9MZOHF%2FEdGkaFr9YC0axDOfmHEgyuG3rDjAF2RHOtS5Vz%2FdxMSnpilE9HFfRcczn75ePIJETj3USI4MtKXVCEb%2FkHkP1HHcVDdSMB3l8ebiYvwXlSEhJXwUYY8lxBMO3jUNWCiXNf%2BLswTp2inAQapr09zyeTsYkKcNBqHy1KMz6l%2BMwnHSvqylJffQjV9LSkWZmUsGwY3eNkO6kWb4TQjt2pDlNz4cAIUS7lUKLOhNxyBU1ifwywU5cZJNPmr404bWZ%2FpahMp1AeetnUTm5MP5HLML7d3MgGOqUBsCvBmMogOCYinJWApmwsnZRFxBFEY%2Bes793CkmmAryXXE6eQ%2FOyeKpOoA9QcCycYd9AgXa%2FvOpQnN4LUlS1Adjmvc5KxNVN0ZRwXlHqtI4QQCXpUD1YhZWa1Xw4StRUVGlXq32BURKMwFaad3rvlEiK1ySHc1kyvddUyTZy8aRdPj65hOz610eeM7thrX8Du7%2BNUSGvcXNSADdiktYbUi5ZWyouU&X-Amz-Signature=f1e9181712e30c290a186d173efdd6e6b78f26d2638682798b96e955719bff1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


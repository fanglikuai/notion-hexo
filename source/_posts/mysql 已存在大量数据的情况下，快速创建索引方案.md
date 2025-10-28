---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNBYOSXC%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDIXI8otClm5WSPLbVuE%2FNfd4VEddDmYvNE9PRe3RFtswIhAJxPoM2l0xfm2yNZFHxXwc1R6fXf0vSmPqMjieQwUzCvKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwdb%2B0p0GHG1kEUUZMq3APvOzLLyRYVNHstvJ%2F2JnyBLklVK4%2BHj6a32aNzqrcnHOd2Q2kbQK2W5UQzZq5ByixuX%2B8PtXrJLw1gjX1Qvpm%2F%2FQcLkEKz1vo1PpSga4oLtKtqsOj5izjfyhH57La1XXhC86PMkMKy1JXk3%2BFt8N%2FSTJHBr7J%2BKimnxs6cDrgoW6qU648phbWQsL8r5gPk%2FL1ee06DK0p46MVNDsqCgNgF%2BJXDHTSpJ0%2F8yhiWBpIFwl3OpySBjqPZv5QG7xc4JCIrHyfr055FDE7a%2B%2BiExddDYF%2BfoknFr0pTaS0cnkSNTSRUAo%2FptYt2wAnwI25I9cvMc0V%2BZESrsjBWcsfNLam8cfM0PP7l9IfrqYmD2IRNlgbx%2BVeWjUUYS7w74KBrfp1KjM9RGqiqOKCu3GLE0Uxf1IGnOP3KnJPR3anN3RZRXTxS%2BdcdMjO5vYWylzRh6Q5DZODh8ehftRQgg4sUJ9tYJSaxDDMbVHPeKH4MSkO6w4dhyLgzAmBW%2F%2BHagKV6V2CVodSsnHBtbdhxo9kTNJnuIL9T5wjNk0XE2PzBZXItsfqwVcVc2%2BqayfP%2FLEFPZ5xtSWjS4WGt9JcXspkD%2BzaprHF%2BEoXZ46gWJu7CtuxyTt6EiWmPZDF4t2L0ezChvYDIBjqkAbcG%2BB27RzJ9o7t5tQ%2B%2FzFYgzPbp%2BM2A%2B3jcwND177nr0d4867LwzD0hlQyk1D1XXJdRCT9uC5orHqrVOelKLLdtZWhZPWbtQq1PB6SEYDWFEq8lcHvOgUIP%2FfdLX2kZs3s5bQp%2ByDNQ6RrNI7nvUA7B05pY8VKrSvUUiZjwpMXwioF6g42s91laFXof8SOYkyTQ%2FOsDMbCCx5mqfcCeGvDWdKjU&X-Amz-Signature=47f40d04d8374a6c10fba67d509f49dce901f9b8c952bfe43eda08e1b0994c0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


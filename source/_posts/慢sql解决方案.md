---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCJCHDS4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDWkmXgyrBwtpOwsBbRUED86wpdKDiUbZcy5IUgujWoRAiEA%2FfCUnzXb43smiiBpuTsNu8fZm906xbPbQFQ7BLrEni4qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIjVmYGG9oNhlzsmXSrcAwTPKsmPE6DWye0B9o9RqK3%2Bp89EFiZK5zogZwbWoQdnLbW3zttlkoLhQMbwR%2FZgIAtJLWrb9VH24cYKSxYlt5Hgo3wkukgAjf4HgEN2PwXRwHZ%2F4jYmC0N89K2yF3in1zG5K9RX1bow5cdNkTEqNlXn0TBzX1NlhrSyUYOfdoJvnpZ6yzqGQt6RGSY4HD6BkRsWhWAU%2FN09nKoay%2Bdc33ml09Hi7Rm8NS46opzxAznJcxIq%2BvFjpvk7mz1I7X1oyO70%2BHpOf1354vmrhcAcrxCrQl6Mv8lwQePUOvSe5d8%2B5QSu3GIXfR2CvzTHHP4eHVcG%2FP3zAThfURBqdjjVaDF%2FC0RIr3G22XJ1OcNYA49d8OcjeS9R7ER0jbgfyv9eUYVFaSHjdb%2FIx9Mv%2FzmGnk5%2FuoxMmXP7JKRfMKw5Z2DVZrYtWD48Uuic4iZk41x0Yjm4WT1Su8YZaDS6FpuVmmK7IJDt2i5FWlgWp7gsqWi%2BdhYQQAZ%2B6xs4PRV0Kpdb4oD4N7qdzXgR6rgDyQOOSbCCfrdO4AD%2FEhOSapG2GZLF7g13df%2BgaRNZm6lMRILeZfjdr1on%2BvvVkQX6AU2rahQmvHjJHqVnA86906PQenfr0gO0H7tUTpT3ThCGMIeJ7cgGOqUBW6Tq%2BL6JdGpq%2Bd0Htqqs0sRIeJfWUAYl8h1bQqI%2FK1lAGezDPUpNf785HsNP8i2n7WNaSgbcLObhqQchbJxf57BHu%2Fh4qd82%2B6BMWumaKB0OLOm0kT3VZTkrvxO%2BHbMoWjAf7Q8oLbjwkJUZs%2BhpXJLJj8v9C4VCv7osX14sBctlrYniB77%2BzfGLskdy%2FpML5SnPBYsYUZ21WEbzbj8qxFUfPTHw&X-Amz-Signature=867260d183dc17bdf1e1f0fc35d52eb3394c9d7fa0f6c2bd3f2f4bf86cd7a420&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


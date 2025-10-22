---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQL5FDMW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQDemhQnmDe4v4AmeOywA2h22WWtP2wOTt3%2BWo1jGqsZyAIhALMu%2BtF9XgxgZJSrWPGIY%2Blh9Oh7fk7nJplWLYtAan%2BOKv8DCCEQABoMNjM3NDIzMTgzODA1IgxYIHxC5nl%2Bed2zyMUq3AN2Ln5Rk3xlNm0ZelhaTZhVazEmn82BUdRo6M0bgThQ1E6Yz8wKiWf%2BPSiTW165h95PqQsC1qFE%2BvmvtAHILyuEm%2FJ1cGDyRkoTKfwHsfattVN1BJvfP53PvaEYZ3M4l8Kjv8prfllV6RCpljGoFdkfbDGuVwEoFIhtpBnxUrp9dLgzMtkMerF0RTnVulLPnnmLP9rEZcBqjC7PQPSUtsu7FQn9T1yja%2BoPR6eghdyYwoKZ%2BqcCHKAS2LHdTIsLz%2FsJLHkeXQpAJrWduIql%2FsF%2BIPiV7V9k4s%2BqK%2BpWaqKfDgqTXUcMG67H3QQ%2BBJfh%2FnNJmRsybNOo7obpsy7FlvjRDGOTeM95wGI4xNtHoCdYZeu58JvSYpp62uRyxMW2H32VOF%2FPJLU0dTmPtjvFdj4BTcTnbCxRhUt43xzLZHTQLMoMSIpWxfCfdcSbglJIsO18guSiVk8ljxil9nwacquvUAKcQkdm3RxlbIcfms9biv2wPpQMHYlYO%2BeDGPaVkerGBVWoJF1xh3gfo9Q4mpvNDk7cL2PEz7P5SsnZ3ejUBvlj3yzagh%2BkR8u6YG%2BhnX1vaDRJS3X30ipl1TTtmXrWLie2CL%2Bo%2FnM64YtFC7p%2Bhf4Oxb191Z%2FdEwcbgTD2tuDHBjqkAX2yUIxo7wMmbELu%2B36L8i9ey5lgILj5HRwRBoQX0Pz7%2FbFOzFLMLInAIO0wGVLGIBDMsT%2F1qwlr%2BGXA98JJCGUTQUUtQi5do5UsWXlXva6sKpvOBSRyT10SOLOgKKhVPCPwBxe2VABt%2FFDRLv3WaRs8dqAFf48phZgi6YeBAEW67VDhwgVHZLrAx1ucf%2BDl9zItDhGbALhtoK8FWw7XFnUin9TF&X-Amz-Signature=7c2223febe76ef5da6d6cb9336d2bc52fefbd25700b7d03ddef740085187ec39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


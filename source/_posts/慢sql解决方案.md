---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDD2LZBO%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCL2NEao7jLZigIq78ypr%2BHdMC3nwcsVASunjoEflaqQQIhAN1pRAd8jlCdm%2BVN2%2FpqLyAjSrA25gsR%2FeHHcP4p9hhsKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9wnnteIA4bh6S3bcq3ANwnYA7dDjX2UKNTte2s0Q6l%2FLPgym4az9i2AdRipnNWs5M9607TW5cQq1YuWA%2BJ6ZxifM%2F47MfIT0F4yMLcM2WbEYbhUaSGtxsJXBU3GK35bPhRFDuJwVY8iy5466tHxOUjkFbYOX%2BQwp5bFvEJyguxYTMqsXtGds8O1AJMOhzVIAPSi52cngt6oNnJGrF8FG2o5%2BjqI47GNvAK0hQG4MdHZ3Mp2XWDoHDvmYTz0huY4JnEsR%2BesrrEFYuBxBvLg%2BV4qtMK2VzrncP8NFU8HdMvuafP47kxzh%2Fi2L9tPIjTaH%2B7ohbs54VOJ14HRdmePbu%2B6enIp%2F2UjPTc1ZnJuk7NydOM6VZbZlg29WqGFPpL%2FMVNNEDXYmzvohmZrp7GxK0PB66oUCIzeaCfM8Y5WS766qAy7%2Fm8aUqBygjV2F29s2Ey7M9GXVDXpiiPSg1IJU5Iffw5bboVDqLIGIy%2FSiLOlUS%2FFq3we2Hx783SUY1QYhxlVyd4T1Whuq3qJIKV01gj8%2F7OIbqgHuxppLq8kKrehXPPM1OKYGu4bP2iGd1nB%2Bwp2ehhjbnXTatzTKT3e8TwvLe%2Bc4of4nEJTSHkUprOH7LMX0uZl7gv6hMvB25Xegz9HyAmhV4ny%2Bj9TDb46vIBjqkAe5UyzLwSMB15tmg1Uodb27Q52ueNshpTgUTTqZiLAG0iufCSFURwzdJEkKRArg6F3P0FfcU%2FGmsSFC6O9YOsrSFqnl0a6C5MwFT5eXL8uFYiyPrlUE6bZdaDCfbSLXXaL1RpRuP65w6t9Xq%2FAh0GVNVrFpuDhxVmiODqssUXbf%2BmPuv12zLW3IvnNxinedmZ%2FFhFRmtmKCOItwvaPBvf%2BVqBTRt&X-Amz-Signature=a9026886282d1d95b0d4a4f715808a9c09a1e0d6d92b43cecc4a17aae03ddf1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


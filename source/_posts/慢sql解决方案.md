---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVIY3SXV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQCqyi34dchLgUs2t2tQKsdJs0k%2FR93YjnVhOQIWsnCmEAIgc7QzoS4G2vjzGRcJTVMmIG%2F2D9xFXLJwbuYOZ9kgqH4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDANEN5Up3q9D4SM%2FVircAy3OxsuM3agtrHYeomRQObk2aasqx%2B3Z8SUPKux0PxajscL%2FGkRfZbkCb6DYpp4s20aHOUYj6ZuIXPVsxxl4asXkAy1gCVVa%2BeXijt7nYFEjPVEeQU5El4CEjw%2B6oyiO%2B%2Fa0p7fRr0eGo7cyIL9kTKBmmg2sBYuCLb09PvZnmqskiL48VFUJ74qLBw8i5Vxzh0H6lcc%2BLhM%2B9WwMiEzL0y30MuqS8B0y1nd%2Brd7lG%2Bzfalh0GzE2ugVp3P%2BuzQbRTsg%2BRjYG%2FLBw1uwm0AzEGq0ilNfCGSEaWJnVyZlhv1Ajyxh1DASTep5wuzEiU7n2R%2BJTo8hRUkbtAN%2FSz5nbmgtvO1iOP89acxUmwHd5YcYDR8zdkty7NmmHGEtcLGTVocPGqRjPI%2FCUeJ21FFrwsJRIbvR70KkGyvg4zVdaBk9xlBC0BitNM6xdUm5eeXs%2FikQGHuiD%2FBL8yRd%2FbjqtDos7oimglqYkyPAQANCvWf1k4M%2FkubfLlcORJHCyK32aN2uFxia1PSEUCxCo%2Fd1aC0RPqH9RpbquELL6CMaoigRJhAfX4NjHCdfV2sC9WF%2BHUOFM775VwL6KA5F9gGQHXjQaFtLPU3YtCRzIlK7E%2FKh3j87GtPVRAentsvGTML7nuMYGOqUBANtsNoGxFFcaOX9sTu3WGt%2BA%2F7BXxtkwgbu24Um19NkMZWo1SQ6zNJfhHi%2F3LBBeEBPRfzdJfdP0W3wkbF9R%2FBZRgM8zS21LwedzgEJ4w5VIi92%2FusegfsSpw82v9FGvJmTMzKWMImjJIPn3S%2BM4Wnei1UycYXzPcGeafBEbkH2S0%2BugrEFuF22wABqm3ugyO2AB3C0Y1CyvoMjQFWv1eN8LYsau&X-Amz-Signature=5dfd9fba571b798166d91a5dffe0bc86c238e6efb40cf4405024072f48d2f067&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


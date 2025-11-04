---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOIBM44%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG3uyc0xtU%2BVIj3nxNYDv7SyXL%2F8WhGNVcaAIRRbR3wxAiEA0VwdtdDjrekx5j6FQVklVUmz6H8b7P32qRcz2NUWvz4q%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FK2XGMUVcVmOoSdSrcAwIPiN3%2Bunhskp9Rr%2BwaMIQJRkQS7WnZ4hb01uPJQjh%2Bf%2B%2Fs4x0OTbJsyBDTpbYnLPaGDvL8W3SAY6tIXw1tYxaA2XabyiVtAmRAoQYTtPxSLsWc7Hh44BIrCfp8NC%2FVcSbqyUQ0TQejsIGRGbC%2FSB2Pm2DhnftVo5wb4t%2FX90ejxe8rZEVD5tf9%2BPx7aSFERPCphHMqDo4pkgyd%2Fd6VTB4%2B9FFS4bV0Dc3krCPj5aMgSdNUSjej%2Bj6SutiwR8D2GyRQIBkhlpiVYOAI7esT8PyZ0vsDI7Gx%2BUxwSxO5yIFGiL%2Bj6F2eKwVqs1qCIECYzssfHGUy0Nfc6nlGcGUF1oE9mzrUkeiisKPXMeBPX%2FJWhA2AkTjvbztGa38SQ%2F2wMh8gqaTdrUip3GtUavQbqIXm4b1x01WVX8cv5t9VI0318rJJBz0vW58d9Bjx1BenycaYXrS2NEbowk7owL6UdP2S6S9F0lssRHmQ%2FNpfVNJEWzcv9XRdiajUxmjL8nA2kw0rrpGDLk9J%2FcynvdpaP6MRklKwv9Icxr515RhlHwGxfQa9TXnzwLMttuxBnAi6SWTkMkObOHrN3v5W8ChvNw6nMcdAzmafTY4T%2FerKjJAIVWBy8Cykw0aFAINZMOv8pMgGOqUBUAWVcuBJlZH83BSJ5O9lt8Q7yYCN9MV3DQT9p1jzq5dNKme7WMO%2BdiCmSRsQwJMK2eul4S%2FfanhT%2FIYAo8jWJMGzqz9wm3zGDqzUzBmZDq8lGx9lNJD2RADQwGPRy9it%2FElzCRM6oTHcqJxfFbYPbB%2B7W5K0cw%2B7%2B4tPWassl%2FP3l%2FYGVDSeuX3TjS01GEMhsutPzYdgVjmzsVh0GUdgtDESQJx6&X-Amz-Signature=679d197fd623e4d277078d636442d862a21b0f74d49e7665476b4dd875d217c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


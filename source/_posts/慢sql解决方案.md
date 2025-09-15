---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU7IWCAG%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T080723Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDZh9HLEX2ksSHnFshVtidMn4phb%2FVcVDIDDiWA3WAwcQIhAJYkYmx8KAz%2BBJA0IR53u5NC8XQKdrDaoqL%2BKl%2FhFMzEKv8DCG8QABoMNjM3NDIzMTgzODA1IgwDTwb8PILZ6oPK5foq3AMIBRoqMqWfbq4mfeGeB2iNsZAQ5jTuZAkAD5GZZ%2FJN%2FTBFWgIhgrbbRXIOIb%2BQRzQsabc81wzVTFx%2BOqeGOTwyDcGmQEG9b%2F6FpB3al6K7Yj13fZbRMa311YeAYmcLvWUx3zr4D1HTDCoy%2FUslzrmu5YN7WKZaDySu6q%2BZsyYwneM4SAUk31dKx7TU%2Bx9B3%2FaOQGFq1x73Nat49HQahHdlXlB8iBp0Wzecv%2BXlCCEn2Ra1bFepXTPozBC1S9X0038IG8ZZlzzjfIblvKbebdxlVHVpI3ALDcDo6LaGwe%2Bd4pcRlsbIXOvhBvJl%2B9xQK04ioHeDQx3QUegCgjETBT03m%2Bvs%2FPdMXEPLKfvoXEp4Ez2ZY27bZLY5JCCh%2BP1BYy1YZQJ0CS8XDAXFwTlXDRdhrYx8pUxK8eAQwhUf%2FUj8%2BUNtAS%2By6LHZJX%2F3EFZ1tmMcTXt5T25kmm0v96pVQ5mAew9AwPhUQdOGjQrcOczfH6X%2BTnYn5VE7SH3cQQnTLwTeZvHvOdmJLs8hFybfMBEQpBxB6%2B2x8uToRwELgZ2wvc2vKVn1Q%2F7ymRJOhE2C85aYqz%2B48BiW4dau0ORPMgX%2Fyy5aI0SZ%2FESIS5V6FRVGxPdSJsftkIsSuGFtYTDc357GBjqkAZ3yvqVQyIeguvtSKlWEPiop%2F0p1205qMD4frxbdpiDZNyu2X2Pbar8n1BU5rj7Ag4q8ExrGfiW0IlZVfgqtEDfiCv5osqfYHHzJ2Xrc8XuM1sRKDgrFf1sYbFh6MReTvM3OHt5w9zS6jvWvBxmhXPGCKWiq1QckpLtwN8pcOnVevGM%2BtvgFXz5Mu31yw3p%2FhvGBE7YMQ2FK0%2B4xvMeX6hOW1veK&X-Amz-Signature=700ed7d3c943022e3ceaff8c86b5358e3c73ea869bc1d534df001252b954a839&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


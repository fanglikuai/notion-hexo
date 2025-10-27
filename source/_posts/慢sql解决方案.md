---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMZ5ZJE%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAnKsel3d1bdtDCMQ3mYe3U5tusTPLyRLCPzgvmRlMwAIgHMxP7Trr59hfbFErfESfXc1sJ2t2AqxHo7ybOe%2BrSwwqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB6aiTvqPYNMczj44yrcA6nHYTdKd3Z4eGrKnzoPdV%2F%2Fp51ja1M0m9rAeTLxlk69Bw9KbLkxu01uSAovSL2AseLFkucvzqZbajkxM7MdkL568GoZvYS7e4WtNMtVi44XcUlumAgvxdkdVn%2BoPaz%2BG%2FTlgP7Gfa%2FMYCVCV1GP6P2DRdXba7zPVTN3nc6uAv7geFXh56JdzPhVCkRNrRVOBbl3hguRfBAtIL7DI0o4kBgVukkW8aYw4jQ1SwjiUtUZEpaV4zyvBq3vGLG9bvCDeilD3becZF%2FuRbkeUEdTKbaq8%2F3kJO0vXdznTj%2FunqE%2FVJTFJgc4KtsuUmhbXD3D94fsT7au72CkvwWhbfBJcD416%2FDlPILCbW9YM2GIaGG%2Bxysfn9siPn71kg24ulBkFTwn6hNQmw4zSj9YwZhemEIWjx%2BkWQsn4FFyydxfiqpTvuUV5%2FE0T6T%2F7v2cA1XwjzIBfTTSz00AvIBJMv2m2YuhN0rbhUz6dF9FHlhD4dZZIs4MMozE%2BesC4QygNrQReb0nC2MGA%2BGjlQpeeb9b8dMrSP4NqXVHbg5Ip%2BjJK8Bg3CzUahJasbP2ct8izDiz%2BaAJnym6hWn14BBuwu9HapH6YY8d3Mb7q6VpffG8ThyDXcQqy0uogERI7m1nMLD3%2BscGOqUB0hEmwbMxaqvQRaWBJCVMFf6LwUwVY6KWAaml1OqAbOuheKEM06Am%2Bfk2voN1nPEVJ6FzJyTOiSn0em28koHdyUWzPxXB9sj%2BQ0%2F%2FFVjwDkRdAgvUgxvi1qr7NHzkkGJzhOxki%2F5EMdTq8O%2FICg%2FJhyoIYRxw0%2FLQuEtnZuPiLL0KMScMhKnJFaAsAlaoOUY6LD5%2FqoMGsRrsIAq6irLRVhPI4z3l&X-Amz-Signature=4094632f40f672a9efdcbd2219ebe18c2ed06d45e25257000c9872c251f121e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


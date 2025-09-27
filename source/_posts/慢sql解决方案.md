---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652KTZECL%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDoyzbYPDW8MtIHoOb%2FYWp84rJ5INwEULFaMuxu6wy0wQIgMn81sSH88qAcuZcFIpL4xP9kZB08z07eODycC6AfGNIqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQM0sN%2BmlD6lf2NHircA6enOwQ2DxhHmdCCo0gTIVRfNHK1OeOxNscqW8vdpGx9x1BRtHjNINBR6hqI8pj4b6effG%2BTMZK0KIaVtzgG0Xy71Qc%2F0AShQ9iWTr2gpS6EmhsG6ZiYFfMX3mbN41ML7hjieLZR8WwzC%2FC3WpKFeJ4xICe%2FDpdBUSnt498eCJsO4Z1SNODzwIel78l2t4XzlvUAQMkXGeS8BJwjNBGil45c%2B7ZShy9BTXMkzlHVvovgNDeHHzTMKiSDqkxgYpOailDOH1LQ%2FMs79lJAWTRVLtzeSm%2F%2FhWIuhVj2CX6rQys5tcwVJS0i7sWAJXgIJXeoF%2Bq%2FRfeQVSbqJiAUDilrT8ovdtH%2F2sHY5KltSHi5XmNqK7ILAGNau5Psdf5S6NGxh38d2wSDIyKm4BlajqBpBTJa5Xh2HKqN5tL7ONe689j0Ao%2BYaBRR6OnWGMshw4bmGoCRpodSncKJGgUpqDTNe%2Bx%2FbtqzLH0jf3hAdl83HK2pVqTPjun%2BoxxK1unDcNhuOv8jHgjJp9COw2CMkZzFrhM2av4uhoRA%2B5mSMgNdJklxvKzO2tkIQp9tHqL2nuzPFcUzbz5sLgtnXXVzhsrBdFhLwkq7oXFWH%2BtoDqFydRjC%2FYtdxyDvv87QnRp3MJez3MYGOqUBGKDcehQYHTSqUzgp%2BCErlhkfpmi2K901Sl85oPQolQuJCN14xZ5CK5BZklTUmrEbV1pqS%2FXNvUta%2FetjMcAPGu%2BoKetPUB6A0qkoYUutg7AbAezxW9nZMvprJ%2FvWycV%2BZHGwckcc8d%2BX3m5JYxh3gpsdA54taScQIysUrt%2FIopTr%2FTGlzuWwqqzOntrkEBgK8GmNMeU7yFMBkMpcn%2BkTjiNPHlrK&X-Amz-Signature=6c64e806b7745694bc3e7c548e5c65ae6c8a8d07fe8703d4eb7ad80c2eab5456&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


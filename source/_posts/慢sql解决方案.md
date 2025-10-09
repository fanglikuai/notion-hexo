---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ULHHILK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC3NLLIX17S9kj129iUkATWdMw8yH%2B1h7ZhAs%2F3Kt%2BJbgIgcTUJqBGZXZIKIIjh0v%2FR76a6vMVP%2FMOPH5Ho129N1nwqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN95T42TwI7mpntV%2FyrcAyzz55Qs7d8T2MKFPDE%2FtuT1zR6oZvDpTfPBWRKNPejlBzaKiFi2hugsVO3iK6P%2FR7%2FNW4gjNdyiYK0TG403b48GylSJUabUHKH%2FBjk%2FhSbytQNiIXj%2BxFahTzWCITMyBhFCsZ00LbT78dA8%2BKcLWKXkA3V88KzY0zzTk3Tan43nJOn8uK%2BNbwU6v0vCQ357u5cEEPd0wHA17%2B%2BgEEi%2FWKD79Y5NAFo8JzM3SIDVjaq3VU4Wh7Tjsir5CYr5kSQOfGyoTOdwHcmVn049t0XNPp5gYSTAWLNiaylbSXuCCa49bcYc9hVZ1lNa6qukWWSPY%2BaJNjOCR6EZa1yO8u0m3P15IEf%2BSTa1mebQcMOdWkvVu%2Frmud2Y63nJFdqJsN1XOXWRgHR2J0G9ifr0cHFBQjeGIf%2FO6nfOn1khO2FTQr6gG3PXglGI6Go6Kh46qv9u6wkwJ945gSa3ugg%2Bkt6oU9g%2FzUVn%2Fii5WYnnh2QxzSrW1p2AIf%2FK4xlQ2a6zg%2F%2BXul824hs7rsKMX%2BPDf%2BWl%2BedhRYptYW8dV1rnZdTP0MF45k3z9a%2BQMNJ23V%2FjhgXQawWvjA1GXVVD%2FMRjM3ysK0tIjZzbZeEs2B3DUvA8JUv9ld3UwoxgX6tjkzxQMJfqnccGOqUBERHo6fYa%2BUTOqvOUc0UqWHkerl1002tK8a8VuXa4q20KY1CgMtpPX8%2BVzGfbLZ%2B21EA7sHTSOqBA2fkQ6I2vZoJ0d8bK8%2FuewNEB895vCW9i%2BEYyGsKAQ5p9tMM8mEQsYrcM1haIKE37PuY%2FsrFpf7IB2qozCSo8rtkcT4a7Ok6b1kfXuf922ExcAiRp%2BD1dRd4dsU7Tth%2FWHlMoqRar%2B5md6I6V&X-Amz-Signature=97ae814f18c01d150948b2344afd0241b2136e48defa30c6557607c6546d4eae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDNGTPQ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T180052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIGDKvD0g6R2kXfEB593FvjR8VLEYY4MOESLKybGHIPTDAiEA4yuezhqW%2BgITRA9PLcpmbF%2FHU0c%2FOmtBtXQgzLf9xYIqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG7uzAvn5%2BxFPjiGoircA2S0fthibS2HA2AZUw8%2BbCk2%2BeIyXMumEG%2BrEo%2BAXR4wMQE8mKc%2Fo0j9QA2GR1dU0QmwpvJhSsUL4IrqHJOX1Rd96vOK%2Bl5uS%2B3ODs9RRDVh77La2J6uZBl%2FpxEuCPpKn1a2BkdNkbKpiz4ed7daJ53ZB%2Bcm4WbVlKAFw8s1Phu%2FJ2hyBkY%2F343qo%2B8Cd9FruUrl00OgW81SEdqmhaGMUS7OwDTphcjv13gI0LFVNMFepku2V%2BlmAd5uIY8SpRtNAvHcYfNJvxc7luviqdagsSUwAP37PqKBNS%2FV2nk7XBRQte91RhoYysr2eZ50lCnl5dlXjESKAvFG5kLESBWnfgy2hfO2YGEw0YG1VimPruO%2Fi0cre6%2FvCX2lVHPXYr15YbJNa8rCgCD5LgIQxNqvX7tcR3hF2R3l5dJ7en34G%2FMndI8uwxLsMl62r9WnrnKwbQSLsMH7wDI8Ud5ZWDxRhyrXPwreBt9jHZ5oeYbr9BH%2FTTBjHcR9z4hne4ZtjWs6Z8Qg1SpQgoV1TiUSdFwRW11iPDH7HBl5T%2Bc%2B0A0SweevAAYupiNVQYkZVrsC%2Bqzrgj%2Bx%2BWYQGaVqcpDt%2F3NGT47BbdILRe9NtQWnYQhksf7BhuJsXv%2FQRgEki70ZMO%2B22ccGOqUBSJyJo3fHb0wDhqW8OCfKhk7H50wf%2FPFzz3ndEGzae4%2BEbR7AkFdOo1vcHuJimeIfi49Zd7xffLerIbQEbRuHxjgz5vlpfm7ZcBDdT%2B9DCWTwPxY9tu0Zzv24TytNNhIs7kGxTqrl3mXxfuUzXxK0QAvSEaGvyVveCtMBTAjCNlTsy%2BQmToyS8I7XK%2BTrfeAAP1QAj%2FWFfEaJFDQttMWFfNvB5%2FbC&X-Amz-Signature=6c97ce5f28852c420140063796af0cd0edeecc71d4b80ebccbca244557339940&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


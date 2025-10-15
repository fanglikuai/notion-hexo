---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNXDKPJI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFzHRcAvAIOJZEaBG%2F%2BAu2H0BmKDF19GESDgpaiJyiY6AiEAwZ8cmM7Erz9kp64T1376WkD9y3NgR2q25teL01sRdvIq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDEgMw4WbstlzoYjqTCrcA0dcdcG91BV8p1KgEsEApbiE96wlgheq0b9zjLCZ%2FdaUyffDaQuoihmnzO5Dz%2BRTHvHvkypffqkrW%2BDOTV5sTjmKBZhsqZZblcG4CA%2BjYTcmOBnFY4vJUZRqdU5wA7BYAmL3rNd125yOQ%2BuTGUOL%2FSaA%2Bsebb9RiwisUf32yd9jfUCqn3O%2Fv5BwC6CGGlit9UwJpklURUjQaT9dkGlyqBNv3bLBFXp7Dsmk6%2B3r3ImzNaiJvaZu4ki7DLj3k6xjCzF4EJH1FNunJToiPaInHOQoMdSXKXqx3689a%2BY40xODL8SrQU%2Frmj8rumpupWp%2B25sqwdCzaTx7zU8SVlwmHgZWcdqzQ57m8CeEC1cLpHNfwFfkPkjnUSLHT%2B3Y5acSCA7Y11NrH7uiQygemKjDx%2FMqtVzDOyNb3e4aC8ugM01DQlTCMEpoXC1FEUBmRc3PmSvVyDM5RtIoH2s7f7hWQjP%2BTM4YXopSk6fHoLxxtaMrr0iJrxviyybiQFJ6gQvxRX%2FlBrmqXnpAE%2FVmVISrqpmva8GzB837uZohqMg2Ijaz7GC1BPMMq0Wgki2bwx2wL8BlgjEDIaX1jjfz3HGROjASBGQbnV5%2FaPTKnnao2%2BW0Km%2FBE%2F0%2BkJPmiVUhfMOfsv8cGOqUBP0lp61GNEYyXcYl%2BYR2gw4OvptaqhZ75eAqlRiWFJQvbi10oF2OdOGc5fQekADl%2FAps495OSds3u1ZcjjTFxQFGdxvSCqm93iwoCeBFLBbB4aXf%2FF6UEBo%2FhL%2F9XzwSlsSB%2FCndHV0ZBJVv9y1SuVydOmq0taY6dGAHNSgvQIGQP%2FpLcVecJIY3lmYuIBgDdm9z%2BC53tKcxoiu2JYt5qq%2FrZBUib&X-Amz-Signature=7be71b965d07b700d92c64c3cf02d8833319237995eb870f3a19e47924e6cfab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


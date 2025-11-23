---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32NXOPZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCw9hGP%2FrtJh3zT2CThxv0WlPYpMFYNKpLooaDf0k7V4QIgAggADktrOVOxvrCd9%2FAqoIUYR9%2FMz018AdHNuFw0fg4q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDAf6owcVBdQ1PYXi5yrcA116ZyQYBmo%2BjUiVhPx1%2FcE5Dg3JtNcT5Syb3IKnj9AYpKgcII5lr%2BzeM0I6k%2BXy1BrTExuuiI%2ByRoneGbpACsEsGzXAF6Km8kR9kuHJ1d5zvU6Rw68m4GeyEunFFDzkOrja6Ame%2FFTpNNdiq0czEjjf6%2BC%2BOwA7uVN0Wpp1vFfW3m0ZaaatgEAoRDnRhNuIwTcwSpssbGzcZ%2BFRKqTJ%2BuOSBUoQy6GV1giSKjWg%2FLsrRL%2Bmba7v%2FFPTbwSZqUzHCtNO6rJH8jUd2x1ZcIpIBwambvHByfkqD0tMr3DPEYUJi88%2FggHcVGiNoFc35WBu5bw%2F7VeFOv%2BL8tkaFe2jUSY6gOlEmA5Q6cvt5kWcOjt6Qa%2B5v4IO%2FnyMhD6h1fvXKQqPFxuu2rcuqd8%2BQz4D809gUdiKUBcVvJZDi8o%2B25hPbXV%2BxDU7%2BUyOPW6ti9Jc7cjBWwGzCsMclj7olCsr3YL5xoJZDCvdWi9DcYUEfqlFXH0FmNYQqpiC8NjV%2FfoL4EV9n1x0juuNBTilzBu%2Bmb4IngO%2BaHueMO5Sif%2BGqqyXbQ8%2BnbT%2BvDsXDHfWczWMA7zYrh%2BFfBS14%2FR49bHFHxGGFQi9kgc6Flw49weSaf0o7wSKpKme3x2LpQRjMKyfickGOqUB%2B04ntu%2BOMNUfeFzK%2FyKOm8mviRlWGi4nKL4A7BsAb3eFnWgI4ODgjIMPi9WosjS0pfmvoErpk9844ceFeOphlz25QdEpQNq0nj0LQpaYT8CHkekF9VfkxB6s5FnfjntCe67Wej1lRjk7p7%2BmILYX4%2Bdq3QKd2xPys17DJmMlREm9u6uzT6tFyLJ8alUYePbTR3mCwmt72%2FTUn9iy6OS5RdhgaFjg&X-Amz-Signature=801d6a782592f9d90fff2186bb430755fc85fceb617441b79e3444763906e930&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


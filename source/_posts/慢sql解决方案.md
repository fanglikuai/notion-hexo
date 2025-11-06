---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROD4GWCR%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAl2rZo%2FjstxRd%2F83UhB7Bk5g0VnaDQJtQuac37OINDoAiEAyLCJxXUSYj3Hi9uR8LtibyGNpkMWY%2FB3rPLLBTY7fq4qiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAEDktmlAQ8zb%2FPxWSrcAw7882NXtBta74ZAhz7An%2BrHxIDtvg2AqEqLBJFDicnePLZIEjv4PIvVKdUfEfciSptN4kxOW4%2FeKwJpJ92GpkWam6rXySdFAieeTwwJRjMt2Hv%2FoiCZci0UUg1FZeTBmsWvfyRTZ0ANXDgdcoqSK1iBWzu166zwo8JkuQDomc9EhMf8jPHejHiyyvifWR4QqiIqhZ5iS0vmay6%2FIbKHMEJdOTSat%2BeBFqyEfXDXuvlUb0nkqwgQuxIhF4uQKQAyaYe3F7I04lu%2FyH%2FL3H9fyH2YX%2B2meBLIdsi2Ek9jYQ9nhvYru8ejPsU4%2BLbWlgo6TO3UO8sX2TFT2906qPNaSvJtdCqSRx%2BrRd88e8x8Cfcy8zHRh1BOzkfpLVQP6EwlqbRW2fKcKB%2F%2BDiHGNBSbJg%2BTE1g%2Bzs%2B9jP3zXr0BSnsbYNfDXIecoEraEEopcAo3%2BZaoiXs0jMJVfv2RshNV81HAyYF66nBu9VUCZIdX5BJl4506RlcDa%2FVL0KUBEhvrMZtg%2BCVUv979sCz3cvWzhZLKugxhSw3bT%2Bon0wXeZ%2Bm4SFHgmA4%2B0gfb3HKxiR%2FnB6NK3Q1VlHfyQAoD2pIXgpzU3UqH%2BzqNGJn06m0WNEGH4ro3zZyLU12TmEiSMJCDs8gGOqUBx5%2FNvV65KTqKvVLGgIeZjOYDLGZNBVYXCVrku3JGl5nmWrjz4MLIzG8I0kQUKB%2FQs0fBqejOp4kR%2Bpg23YFcylkoZP6yYMnITlfy5bWu5rrw3uzl1hH3DQx%2B3WdYZKbtAXncXdSQUGu8DmNa2Vk2bEyA7POKAImUWZXxb4ksG4CqVZIqSbocwZBCrpTwBe33zCVMSGTh84JFMJ6qhakbv%2BaiFE68&X-Amz-Signature=9e61160aaaa938f33d2cf50c886a3b6bb127a310d8e62fd888a74643cb37a942&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


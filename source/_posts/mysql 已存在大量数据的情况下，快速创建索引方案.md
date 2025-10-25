---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664P3MBZAF%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmCb36TJTK%2FLGcb%2FUQ3jQUo%2BAAt1WvQbdsUL2FHGWTGQIhAMMJNPzIuvFcKcH%2FdZwZ%2FSSiJUnkl7NWrLZWl0P2VDtfKv8DCGkQABoMNjM3NDIzMTgzODA1Igxo6Bb2xP4PRgTvsPcq3APlQZ2GGP7ZiRTFP66XH0tcloiw1nnUUQHCI%2FD1aIFrH%2FpKgme5hAOx%2BCG4t5D3DEEoFxnXSBkPoT0SxOG4qOSWX5IwkmBE%2Bc4kxzLSGM0CKDl5b%2BFQq5byyCCHLQjIS%2F1IxQxtTQUrtXgaE17%2BrQYVd%2BXAIsv%2FfzL0wao6qfV0e6FZSKjMzcsUp1UEHsf04zZav4MAFjIanO407mZnucaahzpKvRjZPsgBlJ7TwM9fftS%2FP9cPADpa53fv5YCttpBIeQVRDoZ%2BMqubHWzARISFIWgF5HtCH4Q%2B0Iy97Y3H5Pe28zbRAkc4Q7JN%2BGTSzf%2Fo9sqaY9As2vkghPjcvBC8SqeM3TNJ%2BxTGhJvsd1jPB6DCb3iVbH51bVx7At65ArYm4pmuLMRchDevtcS75G3bwDifxRm0cHf4rfn5fPtDfzPDr3mIn6fC5PBHP1wRCVKNQlMBlj3ry04AhpmUSuyB7uZYN2hbI%2FQagQc1Vk3jI%2FYTWNkF6q1JpWjfVTSP2LAR1KwzDL2GMWIvj1vAiAoSI%2FYtxyUfZbznswz90A7xRm8x4oI01rXtGyq5lYr7z0FXZuNkEQpRg%2FuID0H9yiM0IyVxloJCW%2BNwQZGMF0oJekWZt7GHyUGaL6ddYDCimPDHBjqkAfnOgb2WKVv7B27m8rnZjn4JU9tFqiijBIdGLnCm0dGx9llbMuydnPv7R4tM5LOxXTzjhG7q7i%2F3tHBTC78yquSkiYIPst%2BS43eOfgLhXnqbognG5nPqIrxm%2BscCZLzTxzsUmBSR8CNzLJvL1nq0eHcHh2BCk%2FDjZm7rEviaVsrQkGFoaL10qAF6nq%2Fqf%2FrBJ2ULQZrMD1%2BPnzdTZl%2BIhBkHtNqx&X-Amz-Signature=f65ace6fbbac45a71e21de84da3326eb563f8cf80af66829c8383c4fd3b3d932&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


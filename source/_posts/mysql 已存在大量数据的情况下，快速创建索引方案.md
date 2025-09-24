---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBQKXZFE%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICIJOhYeLS9GbDJtYTbjvCpO2mXNpuDs7qFAehYCJq3%2FAiArVh0ps3CZ69bn87Xx5IYhXrtfPbvFxofPwH%2FAEqaa9Sr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMrmqvjfFb7EbRIDLwKtwD00Zap3tvGCipHMkYq%2B3roC2RX7JED9Ogt2J%2F0i7Hh4kAFojb1PcBnC%2Fz6Gs%2BAm0obuDEqpz%2B6hFOQKMNAWvcoe5QM5PYJGpNG6GfloCrLok1%2Bo8yX7vnRLbDFqW0yEgUSO%2Bw5OFCxXDLfaacwPFXY663HUa5Sa83OWtXBgXW6Qaqb%2FjBLQRUNVTVztBnlUPGDs1PSN3TLM8qxalYLeW6Hnl8b%2BEIRE2DahEJrAnCy6atxP6RYJ1QfpBAYsN5yR%2BwHZ5xvCCmz4%2BjDMKBepHuX8Qx0jAVVDLODZT3%2BIqJTtkAmTNa320c43jheBFrBhHhtMq5Ds7hG%2Be3MXLRf3Pc5J2Pfo39a9e3zBVhwJDNEX714%2Fo5fKFTVAsjiOmu1OTXAaD8MVapOcL1hz%2FSLJd0dahUTJNiwKyRgYe3aIpFn5zy44X334ZAOXhiTkms88qfz%2FgQ0eEufy1ioAP8dsNRudiGdArObl%2BJgFJXjtqANpKE%2BrDMFjPPpeKBM1ID2Oq1E3QUV2w49upI1S9ktZH7DiCFiwfSJMvjSYAhSO871N79O0kQhtdNMHGLW6iC%2BdegU1dDTJAMwclmRa28301ZgJ%2Fx5XBToB5O9Ih186bgMUzF0iPiitMd2aj1tFkw7%2BnNxgY6pgFEVX8hCBLoRTASCDgWP7qujIjkIMaA6Kj0uHL6jSXM3Z1PHWu7WeTRAoh0mj8DLWBc8xWYdhAejSXK%2BYTBA3pbNZmmukMXQhDMfzo4dOCv8sgINwSafKBqDbtM4z2ArELgxlVsXvr0ZoDKQ1%2FwELl9ltIDjEn0M4KZD%2FaB%2BqFJLsHUqoGS8g3zwishR8bWb6WmnvsdWseeAojC%2BFeLDAikSejmBq%2BF&X-Amz-Signature=1e3d1eab3373fd3b710fb17cfa14b83d2296499f8997f990c63240338a942b31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


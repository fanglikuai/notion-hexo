---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IEY2PKH%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqP2L42%2BT9COB%2BT0b8pY%2BvEJsEkYTccOdO4rVWTqIg0AiAaTzyolR6r%2BBHQ3qZTV7N%2BTMFZPCg1djhs%2BYqBmw%2BPhCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMyG6pDGX4a2hVh7KkKtwDGGzRDa98dJS7C%2B3AwOlfPF0iSsCjgbDi9m1KZHd2GSiBCGpkn0LPPwEs9QtO6nDz75Iv0%2B%2FzHjSkml%2FDphBJObTT8vBSNkZ1CY5qppvp74EBQJpLOFeGfPQ1j9KYVoQLbHYRW2uBnJwmYWDczn4QC1AQKsKUx9IqHoyMKS7Pl65JtjhGCUPXjzgQnplFrhTVbwG6sTnqwFE5plYZew%2FmJ4LTN7M7%2F4oYs8N2SaiiuX%2FeyijTplye7YozklZlBePew6wyaXC4ep%2FgJBvzMKp9sBcm2gHimXplcj%2FFWyINXiOsowO6Dg4lIvPIabaDdkaEUwmMnTt1J73D5lal%2FtcXn0Mh7Ps5SwydAmi0dz3txPGo4HypPDJceBucqUdcRPmvrwEAacEoRIktwzya6hbuIClsjyF9A3IHxCmBXbDnRD6tSYcSOXGo4g0qgvx0irg8BIYCQA3nNRuChBCtRcINMMAqGhB4oCOuKtizYY9Ig5naImLuYAMiMFBRpi4t2zRMAz1EmRzLPgAkqJ%2BzAzThM0oYNq5eAwx6WFJDS7i0ahpWIdzoWsQUNFdjel4E3OU7glu6KsdGB59z%2BqyYN0Rks%2Bpf3JAvYo8jEyalDreDdAbB8f5xjv9kPUQTlSswpNPeyAY6pgGggkYAxJx56QsD1SawE2JrRjENHDVbd9gBbJb2wS5N8%2F11PdlBaiRcNPU1OrP8E%2FMatges6AsMLYhDOZjqu7ccCUgtFkZuR3T6D%2FNnrRfEDkLLWae4p%2BhlOdzuqTxjrqn47KQehkSOjpD8f8OXcshJmbTFTjCR363opmYbFC3p7%2FdIBHQQrNq8r6z1P9dwyJ23o93bpFPsL7aPXiiqSNiBb3wh3CBs&X-Amz-Signature=fd7f9af9784a1c0eac1dceb0d902d002aa537d4be7e2d82f5fbfe2917b895078&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


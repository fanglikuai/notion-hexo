---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7F3O6ZN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIEuBGhWa2i61YpMYmHTQLv6PGnUSH6BLXZYPNGOsWfudAiEA38XgOPkd8N36Oe8Bmkzyl38a0P3oXJmAlykmjHCtqqMqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCl86LnEFxYGE2bj0CrcA%2Fp6BysZ3L%2FDMOqB7jJ%2F8U2pcrvA2UeYOlgeftiKtAUmCMgwiW4bIsrU%2BYOMZ9QFI3skhqE%2BIK0ZXQmJu5l%2Fgx%2Bhib4Nlp%2FiQnH9D1DRa0bLJ2SFOt0MjhxyTeWc6v6%2BXWiNpxOAWRVoGMF6VZj0N%2Bh8XVMPzIx%2FzN55GeZnBbMp3gPzYBQDFR7CT7Oa%2FQ745Q1Z%2BWUaz3WON4vKHtMOPElD9LZO93uj9MQpsn%2FUOHbOTgsyoudyXyLWt53I8byYOf%2Frpphudnb6YSjtom9V7WVoi22AVeVCvO172mRRYBosZQwNGoSWYOuu2%2BpA390DrCFgkI0rliKWe32fhTjzyUQALL%2FpbgMF6nDG%2FL1sweCNMNlFqFiEvxzVKsUC2eX5Xcwj2P47Qafi3a7IpzyrnR%2FZmIM75hklIvQykOOgsx3%2FSywojYEUxarVqdV215ImIDaRDb37lBflYUeUV6nutQ8oPPOlxeb%2BaVWP%2FRo5cAwkaOVgSyHf9H8a3dqR3%2FgkovQsTD4t8FFI8u%2Fc99BLQk%2FPrEHko30fXEKk2i2sw8Rkt70KUqSQ%2BJvvLiS503TUIlLvpWxzbH%2FZxMq3xTKwS%2BsIuAZxYjVpxZqs6mdy1osqzdHL3a2A0EVPzb%2BYMLn11ccGOqUBOzQfyqrTlAbI9Au6uZpf18neq6xBDIVG8JMZOwNMY1HJsbgroozMbsGGEOMOliFvE8MG9EzEpY0o%2Fz62HSY2NDUlos%2F%2F7jpq2%2F5lVNVbnJ7V7nO7e%2BfjQNZ5vUdhWqprQtvPah0CUUu0lfJYbQSslG%2FsLHYPmhwMTkKQxm1Omk1G2Do5IDumeTlNh509sDPQbrG76FOz9TlZqshiseTzXTvPHN9Y&X-Amz-Signature=a099cad9c06fdd2f2592ad787d3dd874bad108155b90be1e46c78b5dd7355c8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


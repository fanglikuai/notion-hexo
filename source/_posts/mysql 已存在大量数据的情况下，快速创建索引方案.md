---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJBM75ZF%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T200047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJHMEUCIQCoE1fNhWBtnWC0JHmQzLoM8S92itYuQzuN6Ofp5ilC4AIgC3s%2FuzvZwZGaVeJa%2FDKAs%2FQsLTuNOPsriL7ARvQGGgIqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBKCwINty%2Fsgw2xmGCrcAznTi2STrikFVGxLReelVpLUyJLH6NxhBjTJ%2BHEnmJwJnWBSorpkc4m5EBS%2FxL0OEg7T%2B9xC%2FLpLOgGs1Sfxf%2BOdC7c%2Fx5VbgtEqt9w%2FTjvKY6SYIIm5mpoARKeRn%2BATB2lIZSrFQ6uZUHPggMu665zcDQ%2FCrENkNPhPIfGGcDn39NO1prTfhXjWN1np11r%2FwiPrIqQrllBjLC%2F3%2FWMspZ3H%2Fr%2BLndrs5jWaJ2Fjy4ljC4fi%2FsR4z12wC1jvt7DouTOl25jymuhDnAV3sFNdicX6UGwOvKeY9TwgC5n45I8oh7WayLjHI%2FYwSUYOq7rXGS%2BpZGhtVC4aRjl3h%2FuF0eh1UoHV4sK7kxuvdvZ29C7XMYosCi4G3RGZaSEIIWLLBLijS%2FNB9QoxieuQffdM2hU6tCuEj2okaxOUQahcyTpqilp%2BwuwwdGGfEINRVfqgFJ0TYJGAa8iwuFfwQw8zEUM65z38pp0yh0%2FEa1SYh%2B2uZhCKRPsv%2Bz3alFACCn2%2FiDre%2Fp8E1deYFeZ72WAWxwN0FZvGKZMYmTcdIXiqC3kYrdoifmIEcc%2F3VPR9mbV71Ks7mS8qePCdaTHYrkj06jNH2pLrdjRWi0B5xcH7778yom5FfYi3PZc1g1tGMLbh%2FcgGOqUB5lPvMAO9mOE1VtWDG%2BGXTN0zFfnu1S3%2F91liaO9CZWZbPpHqYOKXEVXX1wNOrYQs7rz39OKg8b8RQfHSfrZwzbp5YN9uo4ec81aPhClMIHTRsxXXeK63V%2BxXEla7M6A8QqQUzFNlLFT3NiPy9pTENmNeY%2Bw27%2FTAivmjRCDQN78iZOw0NBUdhiJpHnxtKFlbLynwELYsTgwvmiq121NVAolexkg5&X-Amz-Signature=3a022f800c71399badb41ca01d0990bd084c4332c5bee0ff07c1afbc76858f31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


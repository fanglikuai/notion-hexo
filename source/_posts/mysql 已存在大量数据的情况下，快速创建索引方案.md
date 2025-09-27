---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQXKU3J2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDva5bAS7mEfg90nHdR47SJIhYzkoSl%2F1Q0eTwg%2FyMFMQIhAIDlrEV602NWNk9NT31IkXpLcP2UxAVHV70h1X%2BYQyiaKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw3I3QPehISJsns%2Bh0q3AMVzb0n0NPO6ldTe4s71Hlwdz3fF8CuU2cddbGfnMoVA6M%2BrXqN0Rl%2BzTnBWmsmGgPeaSChFmvQCD7zwTWsjgTUTFJLAGLSJKWKLSyOvFN1y%2F%2FqtWScHVK7dxBDaCUK0HhgLWClQUvJ1Qqfs0sWmKfTznl0vcLJfYV%2BEWQi871EIWuaZdgzxUu%2BYtYuRES07MPz7LefkFX1lSkDfOhQ3yBwr1P8BY4dksLXRnv409M5ke1H6869Nn2gWAlMEBRCFXt4A0hwqKfbc%2F4ECi4iRvVpfXUjLMMRRqt31AwVlhGAX1G9WUmXzZIZzpn3ySMoXR2BZBMYifcUC0Dmx7NCLUoanmDmVJMKbJxkjS0vZmg0aprylQmwBOwA%2Fj6w%2Bv4bnjAoPN%2BarpJ6z9G%2B4YJ%2BW6gj%2BoqukY1OkFpwgqGeByRShksl5JYimXl3oHRMSlJWDhlVLCLN4Pcj0WjurAcxuHJw5ubxeVCvp18eWH51Pm0vDN%2F2Gb2TYQ0L5MCGvaweWJUI9MW%2F1YDSsP9npuAIIYI2z9i9YST023ZvCljcpMGk5KvZo1va%2FnM0fq9cl7Qx0eYI1FvLPM87cZiHT5Il11N6qZ%2FiB3GLwilK%2FSf7XXLfrLsfaVOvPxxEtlwjzjCyod7GBjqkAYgnwMGds3FKAIWQGKhjqOZfHEtO6UDTOktP%2B1OouVJe%2FER3uVzPb2j5b40fIrHxbU%2FBoN2NLqwVmCQX0m8HJRvJIfd1Qm405ooT45P%2BThdzu9ME3Xv0HR8hjpp2qUAM%2B6dARtSggPzo%2BkaH6bhKsH%2BPBvVVqSvQMKZJTs7qTvygQml5qY5jzCL6vISgDNBP6cWbc99%2B%2BzXl8NvD7mkwVVm5C6NA&X-Amz-Signature=18ef942d3ec469ced305de336a583ccd940ef4b0b4fd21b0857397308c461c05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664I45ORPH%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhprBeyNA%2Bbv2319r%2FaPL6pR0ee8%2BnTx33dlP%2BNv2JpgIhAKqnwTKJ91MXVYpdksEfH8jgnZZqMzmU%2BTe5BIPCcZNcKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMk%2Bij14iOCcs9c8cq3AMmMZug4VRimcfRW4egtFz%2BjJx6a0mkYrKIcnmIG9to8vKpMBR22w2vEdxXHav9gcvP%2B1lDNpdnyIpokrVjh6z3IQeYh8weqElHE1k07%2Bbj1rX%2Fc72XpEqQ1lrcFTbjfUCD688qe0ppDN4s7ABh5dJRfTgW3Gp%2BpLygCMrrGbhf%2BJMUBlIodPcDmQLiN09Fiu9aqFWInv9jGlG%2B%2FuasqO3WjiI%2Bl5p8BjpMf5OQHzzHAoQam3BmUUvW9v8fpXGEJeYJOXiZbu%2FgLEiZ73Oaitatc1xjWd4rVB%2FiUlt%2B4tigb%2BjT8s6Bx6OZIL6t2AU5Q0%2FELDbCM0iiIh9%2Blx9e1WlbsupBOe79k%2FZvcKZ53XdRpNQ8UQLO7UcI6%2FfFMEk7CKgyu6O05j0EBm%2FKUADEzmOOJAkGozfFevcDvnK7m5ET6VErYfNE2GjXFL2swu4wceS%2FmB3S3My%2FcSqxzu%2FkN3DGTO0xjG3%2FlP13Aoo4g796iMauA0dVlwgCmqg11mBedANmGPZYaHcn6htkZD2sbqgsXNS%2B6G1coFP%2FRNyx66tffF1Z5N85LSYruvb7ZL5F0TM7o5i%2BfnxYemuVY2jejnNH1vZHGHCJ0tgMN7Z69%2FBHNFYJ4PRQ6FQ9rNIAwjCOxfHIBjqkAXk8IVqfh4hAOsGIRDsDT5AAnM5bFv3gkrqXyzOPwJ0X6AOP4hJ11J2eAlrSeakXNq68FKN%2FFWxy%2Bkn2EbeWp9Q%2FSTmFLJlz8ZfJzG7qH4FxBixoSJs%2FqSQQenMBIVCqT%2FmxOb7TDTqkOSt90sC9s3k0o00WLruNlMCe%2BupnRfg3e7v1a9SKiTq%2BJ3rBVf1L1CAOQIh14yNQ7MMxZdU4hfmRLoy2&X-Amz-Signature=d47bdf5e1b6cb186bf0b4206522f09aa6c534951151a24d7b701c963e5c49daf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


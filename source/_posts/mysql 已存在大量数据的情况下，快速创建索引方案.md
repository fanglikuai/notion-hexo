---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KZIM24W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T140056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQC2cVIuLw%2FQVsp7JMG6OGB13H3BKgK3hJD%2FFopL5K2EogIhAJoCyfs8AE72cEyW%2B1YdouBptzY9q1UAIIMOqEzlh1JtKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxfPXI69cZBDPSabpEq3AOWGM2iEUzL36rSD9jqpvk7RG94y9uFdvsLxMsCI3ot8g4psLBaQ%2BUrFBeAxpj%2FmXgSXkK9nNWX9EQU9sIjsqh8q9%2FU5MMz0JGk8vovFGWCIiVCqN5v1dGbxsTY7pO7ChJC04wkVif6uwMshGyWavcREUNmEsowsAFBk5plildcj%2B2v1AKRsnSIWfleF0MAtrn8SBq3aMVRE0TDspA2uPJJvITQ4dBcUQxOqz87BDGW3gsq3cVtZvF7Oekvwmageur1ak5BEPAhh8s6rlypZKiMLumOvocL%2B4BnK2gV3Dml0V6rHyh6AWhiLGIqkfBYbzc%2FbUf69%2BdaSDZN0tI89Sv%2FL0mBzZDoubNmi53qgKZ62iX9nGpl%2BGxD6ijPlSCVTlA%2B0sdEWOeYi4s%2FuKdfPOSlrXJMdqrLnExPA0nJMx4xX1rxEMyck%2FxLM3DMQoZenMxC2tDUT0Rd3fxaou%2BrPvzitkfswMF3CNrir8IXHeHDgrkeWPBXNKUwyYonoSXca92TnI%2BhPp%2Fn1ghntusyqVvRgHB8o34NWs86XcAj8aOUwCAyxOFC%2Bau01GbADrwx1IaJ9%2BvCbAqck9MeNKAYrsmJ6%2BoXvEJnmW5ntzNg8R%2B1K%2B1cMTdAbTowPYWePTC0jbzIBjqkAfV5MBcBo81yMYNtbB849J8ctYBF0ojeGqcXv4grzuVGW0A%2BgqWY5w9PvsmDK5YsLhfSMVuEtMq9ZP3tOP48N4Bl6TS4IRIniGx9QOHc0seeszs%2B4UnTbE3vRApr2iFJXu8fk9iRLLrxY2e%2BNmC9sUJH%2BbBpX14sy9Sv%2BHJCaKG%2Fuod1GIjyyp3aJuKAsnr5J5CyqycDa1%2BtDTex4FTRDzlwV6GJ&X-Amz-Signature=45684618b08f88810b8b21f76ded6d7c1f564253bcf528bf5c8c19610696faf4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


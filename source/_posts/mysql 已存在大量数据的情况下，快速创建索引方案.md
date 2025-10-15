---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3PS7BLD%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T120136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2FBVmm2Z9YY%2BP9RA%2FKEIWNF3TWdsfikxtOrpfr4LRUTwIhAKQGlO1kHQVJb3Xl%2Bswo3baXXV7TOmbGjshXAq9BRqbQKv8DCHUQABoMNjM3NDIzMTgzODA1IgxTkhZi9U9m4ZbSCbQq3APrvNU8j7AH0bbOY3futwdi4JO7SKlthQeUvCDi7jk7h0SfbMABv9evXi1%2FMIsgTjeEh7PEWacy2K1VceuaK1GaOMpKVgWOtOBL1UjJCpc4Y5nKe%2BbPm0kROyrY%2BwVVCnjtpXeTsLDoo8ZX1caRK%2FwO7BIPkpEGRRu2zQtf9ymkqPKUbIVoa1kshl85bWjTvhVFb%2FpYnqulx%2FIlFNdXD5FYQi4%2BPbrb43gfG5madf3oi0xTKTDEdugsminhNzEBP1GV2p5FcclT%2FLO1BefhTx4wQi%2FLdzFVj42UpininNwZtRW92xASJVF8h6Ab8PgP%2BZIOaZzrLrYyVwabj1gw%2BxNx%2BZwn9xoPBYwKdALOsH5k%2FKfhrIJmiTPjMqjwTidxO0XiF%2FXnN1mUrne0jyms%2BnD9v5jzRz9K0sSUxKBpvtqFp%2F7XpJ29P7IMa9TuKER0mv7mYO9P0at3D%2Boh4Q3ggah8nMnnpww7n8iyKA%2Bfent2Dc%2BpIW2mlVUeZPLrPxtpXSmZjmFKVuv2fUVa4ST7v32JsPvIB9w7maMaYocKKcouZPoUk2FL79G1eeoq%2FpL%2Bdakh7eOJYVkNaF1RL%2BoEiEja0nfByjrJGgm1wetQFYQFFhe%2BazxCz2T%2BNg7uiDDnmL7HBjqkAYj%2FPSusprZ4UGQsaeh0NuZDmIDEKjtBg7prcJWWNN1iZYeIbbx%2BAuuqYWZRANAr0LgsoWjwodfVOSE45jJGsVsbCPZWDnudz7r1jdacaZwZy%2FA6w%2FNHRj1i9hIqpgZr2g%2FUVzyVsogmBNzICoL%2FX8tv9Zzyd%2B9UaZw64N2eRCVJZdpgOEntOEbIiKW7SCAHbMjaR2HmIExMwNbAoEWCcC5IvFXU&X-Amz-Signature=75817543e3571eaa87da7e0a63675b10056ec2cd688a9f64633aca985533b9c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


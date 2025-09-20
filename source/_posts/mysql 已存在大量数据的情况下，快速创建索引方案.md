---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUU5UFGX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDBTZ5a5YjHxPc5CALr6lDoK2BpbOHN3%2BwhUfwXxDdkvAIhAPeX7GRkNYlej2f%2B6UkxSisOkUe6sT6gwTi6Efn6T6TYKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwoBp9a34nvTqkIbNMq3APdUBeaexrHilB%2FMNxiB2w%2BRUWorS2zdH%2F3ImxATOXMbiEgGabXVE1c1cL%2FCpT3lr%2Fd04JNQU2e4JoRtmmcejeKBQNLjgqt%2Bc1CjpKm4%2B0qltW28Ej7pOuQxOO1hlzayjp0g3Hze%2FeQ0WWdMD2sQJdLIdKj%2BW5uH0Ni1eOtITs7XSetkcwm4AgZpGSycRyBpBRmWASbEI3swNwJd4M%2FD4YTU%2Fuud%2BL9ErAu5guD0s8yLkTk%2FN7yBHpMFrbnKnQoRjz6jvOCe%2FjdjrxKqO%2Fwt3eqra4Bxxh9cShdYN7gbKrtkWTqFYxIjnypO7d5FzIgKKUIDahsZPrhR4TybVxSCnWnJxscIifCLok3MLLKwMr7OQvCe%2BKzdaLWjx0%2BrBj6rm5rrZvvvG3zJmRjEmnMCQEQOaXMg288w0C%2Bx%2BeYyMLOuaMmObejRUtiQkB%2B5jRT%2FQMJ%2FYXacseYjvrD0wapGEUizhjn6gAmEt6lXPV9EJgu5qJu26kuMHMCD0NyQEpyCwUPKBROHohSsfPPePoSLMCYrW5SojYPW7Cfyk1HTIfEMPUqm6Q8Hw7CBExI2066fHN4g3eZk26QP3FfaTKu0CgDfhQlIwz4K%2F2vE4RK%2FuICVdRk3faY9Duc4OPuRDCv6rnGBjqkAbsMETn1apvkCWd1c7hSHofM86fZAkvjQjvC3%2BqVv653kjQXIMYRbIaseGmrKgcEsxnP1r1lScVwK1sISqTqn1flCrQOOsTG1NGREDmNAs4U5n5pQTaoJbGskR%2FwlQx%2B4Tm9u%2BEbXQAAEyk9bGlW7%2Fq%2BBr%2FsB7foCwOTakNrHLuuLbN%2FDTmzhoXGDZ2s%2FKifn4tFaVgmpAG3kQv7JI7xSeWabQAO&X-Amz-Signature=eba7b0bdbd6214c9996c7ff8e6569bdcd306f8d519ffe9b3c3bcfdcd95cad4eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


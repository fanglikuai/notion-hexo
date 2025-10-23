---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGD4WAPD%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEMqgnRzSeFT4DZmm4Ws6yoCZgB6u%2BUNA%2BOyeZAsscxLAiAk%2BbTrb2fF7YrJcXZWaJfHa8dqy42%2BMq1QD6bfWfUqiCr%2FAwg9EAAaDDYzNzQyMzE4MzgwNSIMDsL7EAh3PzNE6AqtKtwDvE4Z0DzcbgDTirmN%2BcyNvmvZn8D478hpv3p5%2BYqkBa6V69T9v6jyk7Cl9nrz9QC2NoNit%2FFDdnoKxbKcPCLIa99UnkPZr6ymFlz3zxR4g7bPgLkKliUkvG5JjgJm%2F5U%2BqHiinqSa2SV7E2BJ2hvCDYF9JB0FMC3pr41PXZM%2FuHkr3gE0ub0KXr7iFyTTHNehEuFRgQamJzHCmb6%2FjuQxYEdnxVYJI6Ce4J%2BjyV%2FjrDuV3MpaLHOxdbXbpsqT46rIEc7jTrhhPifbVYsKIwjsgq2BtqZBEmUSBNr%2Bt18L9p5MTmUpqmaOOydttNIS49U6A7pOowEiMsioGCIoVDBxlXGOEzotpb4ZuQy6epFwLJ%2FU9%2FuUw1on4gOywmgmubZfzagNibMQE0LLATjcDaqDQ4ccSgVCmRVBgkjT9Pu%2B70A3qG6554yHWTsnxVqqldSn9csfFUU8pBs6jVWvmiT%2B1uRvS9F%2BsUJsUOySUSBbq77Ff5ZOheekcLm%2FjQXoSN09asDgSEfmf9FXhjLMo3zyTStkPSGU8z0gDim83pRdFGqsavc%2B1VtHMha1AgM%2BmuE1GT4N1Hrr0rE9hISUR%2BLA%2Fs4kyzaQ6JOQaF4QFqZ7h6poWpp6RQwsTJSzy2Qw%2BcDmxwY6pgErirBbN9xjfm0Pbcn8QkWj%2BF1%2BLvSCj4CkCq%2F4xCEr9acRCEoMw3MuWDfszp1mlpZEKkwWd5K28fLHSRNeEmHXJufzJkYvtGBA1v%2F1cu83hMQSw8l596Jd%2Fuojr2kOHHmC%2FekzjjvTin2wwA5xFbxsjfKjwhfVCtd1cdBP%2BGsOhCfUGvcqzL%2Bvi5ewUm0HMhIPLOO%2Fv%2Fi%2FWV%2F371wU%2FZJP0DYTf5n1&X-Amz-Signature=6377045d7e9a66d166102bc14614fcb7c0b0f42fcd257f5e0308c19096d553e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


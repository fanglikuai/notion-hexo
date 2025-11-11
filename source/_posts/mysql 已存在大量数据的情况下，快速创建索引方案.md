---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY2U72HV%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJGMEQCIBvMUX99oFGNGO1kh63oOjebS5acpnYOmCmB9zQK3Tp2AiB9WuSWCwjzqGhhNGugEOFj7S3tQj5HzUevgNjYn3JcLyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjCaNayoq8zFsO7XmKtwD79PhFTZSvvtO%2B%2FuaAj7LhMhdcznJ2BsKR%2B%2Fmf5C7rlwM2KNqb7W3hbhC95wip2A8HmNcWlI6g%2BdZAiM9NHNjwl9yTxpXdFn9sDZaNY0g5PfkO2d5nVyzuMuDvDMHxEFOjXRl1zFDo7oox1c%2FSsSIORv49jBQeeRuJbH1T%2Bej%2BaCiieLLY89k4%2BMoK96KabNcuTpq%2B7I94jSQkocOa3Z3nvyNf1UspkhA44%2FAcUgld3govqKP730JVASfaT9ojewn3m35SyMKHU5Q17WtWXlYUkW%2FSWMHg8mBHcRVSVECNew1crfbSOu8WU7%2BFO2NdBijKP4SRXcCtJY5pROwX7kvmBQx8iR0KuDfzVaRkuAR0k6rBhH8xYD%2FOthqkp8O3ObvSuPVyJIXwmDb3rTqymLgSc3INzL%2FJuBfetFL%2FovDQ70yN641QW2Deh8AVvxdTgMdaAXlppsgF2%2Fx8AGgZDf59bnqSKENWtzMsuHgHdfkOtHuxDRV%2Fka1358T0UQJDCfXm7X0eEmDWvaCPpwOtRfm6rvhz13u9nMkdzclhMkfa0AQpPovYMUHEPrwf21Y8iNidX9jDjIf4NrlbH9YuGXH5waaiW2zlT3pcKw%2F9UEOP%2B51%2BLizT0ic6wErjpkwzKnKyAY6pgGDF9HFtfxjstH7tZnLYqkWF7fe9up9jgKxNu0Ov1WCxNct53Jirj03DUreryjr1KtIRGzdc2iVY5j6nPwHvSMhnPzWqXSQLeTrF8fEr64kYxuBFEprenH75Beu52H%2BXzlg2lC1JvYD7sjNNwVd%2Bie0G25AOv5xHNEswm8YU9YE8S0QSS2ZuqBj6kKFmqD%2FGCCcB74T2I5R%2FeWhQs7uqnUm3h1d5KDa&X-Amz-Signature=d532d292cc6eb3d8c804e89aade8f834238aff077a45d5c98dc7c31218f7834c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


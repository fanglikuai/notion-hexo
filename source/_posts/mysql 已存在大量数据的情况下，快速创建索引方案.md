---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGWJRBT3%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2TAzqfVSdEBC8j7u1gVbCTRZAeXxAHyfcEedePhqe8AIhANmKe9YPTCCMpQ0ipotcr1Ejkc0xxLMxw2qPgcDhgQfWKv8DCGIQABoMNjM3NDIzMTgzODA1IgwDIHNv1wvOhyXRXLgq3APm%2B73jZhbybwI8toH2XahdeE4oV%2Bg%2Fd3GXtqXCNwrTv2g%2FH3heu0FsRM914d5GrdvJfUPWJtJ9Pvib1Tjy%2BjAoxbg2gMjB2xyA11iGVWdyv2ZUrgm%2FYesZfyiw2dsOFmMmuPjC5HseIGtBt9v%2FKXgjI362ETRqRa6rCz8cBckSbHUPJfHOcAZxjDY85FMzklcsztaBMjLrgXFweFwJ1%2FuEe%2FH60aa8gawfWeaCuFDwnEWNej8qBS8LOKkbsttAdUMnviLEBHCRtGNMkccfJkeIxBf9UchQK3m6FNSxIUKp9PRgGUbc1WvdO0eCoxSSVVsyUvx7A%2FFtiontsvv5gYD22qLnQVAafY9viLVWb3zlymIuGECbTf5JT%2BQN0g2gma9PdI2CLIDaZuiEms%2B2xq%2Fem7if9ixmZqofkJdBk2yYcyi73ihTLCUSdKSKo9a5cQvzj%2BzgW9%2BWw%2ByxW%2BX8Ye8SAwRDHuuEh3b80cU2EqSpUOTVHN%2BGk%2BpccdI84Y4cF9lRQ85hpbusgnK%2Bm6beVH59NmKe6yydZHm4qluDBNukJQNw2yJrgYFJzC8TSJUYgiZJOcz%2FCawrb4rW1CrMBQqwWk9eoMKxCPj%2BSzrW7mgd8dpDrJ3jJu8yHuslVzDQ7O7HBjqkASb6I09Qcd2mJ%2FqZTx9UXP%2B6yruY3ZDCe3mWa83EQ9Rp2F3y6cKpAOPkxhzECw8Xs06Jed2pv7wTtgRqGUjrwuN%2FWwKL7n8o80IHjxM8FHa1L0JSWTmWogsb%2F27nQVXRYhAaMvfamrIGJwDqUduRdvUHkBgnf%2BWEU5Fg8NdjwX0gdRbWGFjUmnw2TFwuAIsisFQA6irLhRqv8JqI325TRE%2BDIlsD&X-Amz-Signature=a2abbb48abb224dc319a9a4f8925ba1fa6b1f4eb1f0310e413d59cfca070928e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


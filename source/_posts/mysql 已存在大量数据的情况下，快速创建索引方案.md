---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YPGFG2G%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCdWHjDKjXifgtt8AekZeyaQWDjKvW2TPN2LcqH12rDQQIhAI2wSTjWOgpvmFuJvI2OCmGXpeHxHMwNJgeA1elmjttgKv8DCCQQABoMNjM3NDIzMTgzODA1IgxUln808d%2B6eQXOrvUq3APbYOmgbDRTo88fV2u8qcG9JbPTmv4VTtdkaHoiP6t8b4genx%2BnZdjkzJj3D51vctmJB7%2BfVKSqaRrQMK5qSIUacGlLhb4%2BoQFV2Z9EuGDA1YlVeQZBAebggDJHNbAbIpby2kmCtqWCfM6KS7tlPgTJGCliLz%2BEcLgbiRTARQjpIO2wpNqz%2B7XaZESTCpUCN%2B18zWropFaTDa5MRfhjlovj6MFbVZDpIlOsFfgZtUkdi6OMr%2FPUZz1h9Ctdvrhqlcm6RwmBYe5U7%2FTFoaP1lBfFPaBAKHbFCjCtVn5KlutOxu0X1xvCXIt%2B8fbn9%2BshyF0TRJuQ1J6%2Fhlybvl64XUUWqeWGIlVfLvUmc7T9p9DJ9hD5JpUBEU7ca%2FMaXJSmYueQ%2BgehGY3YQcJ851xGHRYsohMEtgAgLPxI8Y97lfM6vmSoi%2BwDTRHOTTUwWhvVmj%2BrfHOdUAKtM0lUiuQqs%2BxKkgiUSE7tIPYj6QRMOrjvDEhBpwUaEnJJhSosDGcb%2BMFU17R8FDV2kKFPXIOtZSXxDxzN8w%2Bb8VbTk53%2Fjl57dBG5oMR6bQN9037xttVjD%2BiZtrK9VwriiQ1mRnlfKS%2Bmn2rWXjF8JgjxcCiDNxif6rd0oRFRfdRB62cAwzC2o4bJBjqkAYreBpDWl7OYdkldiRSBbvv2X5QJaP7l7m7lYfIyDzbWB6UrXxi8sl0Z3c1TqN4%2Bhij1a%2FPuipFlyMktNUt7RYFsxNRpLaUml7TkdDejsdV6tNn1keKK2X6%2FaSRG5ccormXBIbhk2Q45YCj%2BxVVQHzWmmvaDuLYGpeabLa7DaiMlRXxUX8HeZE%2FqawBFc%2BmxDd%2F4fdHJiVQkU8XTbANqk0i9g8lU&X-Amz-Signature=0c09d467bf3f3a7adfcc66254bd739cab32c71827884ffc523e29d6bbd3be41b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


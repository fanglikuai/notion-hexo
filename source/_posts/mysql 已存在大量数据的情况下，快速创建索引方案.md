---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4NGNP3%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCGYrd1NXUdl%2BID11HVzbYrE4Dr5bCbcCYv%2FT9JyGqavwIhAM7Dcg14zMfQG%2B77EBVdCtFuzBeACsLQ8r6zWqs715kLKv8DCGoQABoMNjM3NDIzMTgzODA1Igyql6yfKXXSFv1dWs4q3APS2sanrRIbVSngodVhDllkUnLDbep1Jg7ccdJ4BKbcdbP3zW1kSj%2FJD%2BogJmaFmjs%2FMyxQ3GXnigsEDBno1I8akkHn4WOxtvJ5IC1olDQwHRkfF%2FFpHH7jrZ8%2Fm4hT5aSgIn7%2BG7LEprfaZyg4%2Be6g60I7FjpCRTKsvoUuWm3wkwiorkNwLcOBcUpqb1p63u%2FzPOKAQKaT1D6EPIWFn6NqiKZlLQ7hAKmMZSKZgUc84lYaJivS0svhRbJfFJnt5OyLb%2FxacJszum5WHJtArTziASacOo4ap4PDhSsmlbH%2FKRBMHE2QLk6pyQZdxoGXd3nzLW37UiT9797QpWppamPFgjgMtxyEgljFxuIAmcIIo5AQyoxtinR0iKRB%2BNZ%2Fd2w1aNhe9JNEOiC152Xnd%2FXsAmG%2BPJagtSRAuDNNWiM6G%2FzrgZlIV0cbAWliLuWUoeJ7hdxff5oOPs6BXHxS5x0j6nQXu5B52%2FixO9IIX3xkuWl0zvhYL8kp9WcFPRRXEJfFmtuSEtUVxAx%2BMtvudzMYiref6huIw65%2FSYLvSq0yRbkiREXRdeRQPuFPlx7LCnmzdQd7GmCmHr%2BFp7etsAPFi2eJhtYqFr3uFfHw%2BJnvoSnCNKuN8UgB1xYGlDCrnaXIBjqkAR6wgBa7hR1ZvwrhZznGaHjPLIB%2BwOgU9%2BMDIUV5eryIiM4niWpN8bvAxRz9ZkNSvVPtb6R4vSn%2BfJ78vh15D4w8g5ToXjN%2FDhJ9fQjssTrExVLiSGcP3gRIkK3dzhdRnhV%2BVZA6sZJrINzoCNvB8kmWnnLnCQV7OU%2BwoxnvrJPZm3NPfnnvZz1o0aVFWqayqRvXg9yeLt88LfuohBOcR9E%2BP1bw&X-Amz-Signature=8422856d8cdd6bf433b78ceb11f811689f67ad6f8e1d60c5d26bcc352424ac2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664W6YVFOA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIFf8MHeRHd0Cg8Yk0BFGXxB18Rc2Ala3kKP5Th2b5yVtAiAa1AKxRTv8ApLKFnugKfTd6YVBPK7zRZrOzl4z03C5ByqIBAi%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpM%2FeC7ZrbkVHexQCKtwDoS5CDfZ2r3ttLaV9aqOvEQ8FEtX16tjDBM6fIe6AxScAgToGBOjFbMP%2F3eF%2BzSfmszerAxYkhCX%2FyzfOORGu5rihRdproxnKoa8IHe53mmG1AGssyTlzeE9xaMVh0nM8qf%2FLbwFH9KqEpOnuQiu3Ro9Y3BClt27m0tQAeVGBPu4JfdePEW52zDoYQgZ%2FqCM%2FsCbYIJavp0lsz09Oy86uh40juHGbJzE9zuqVuMUd3Sz0e77tebBa8QnAdoTPXMqPN2YXiJDH3571GbbgDWhoJ9zP5VO02YHHnvm0AeHcNPtccnzPKH4%2BE0A2OQf4mmveXno6KQp9ZALWh3wBn4BTGnYprCzR9P1WPMxMfv6Y285fKLpjxt7VdvgMRwkLPJZK%2BSkS2aN3H8JRMyzq6MQenRZ4KCAerHUtgW3Ix8pm%2BAilypt6ow1kSlG3dMRP4AegB6TFn00%2FkehE4Yd77Q%2BoZJGfbZAJ1cKSG47PbKmGCb06L5PJ%2BJSrO%2FSp8k16lBpzoTxtPF2U7RM%2BDGsa8Pu3xVDNnz6RgEmOmQUfqCNHJd7nvJfCap3HY0QWapX8mIxh1s%2FAs0MMlVJLmxbtJFrGPbeJWno9hhDWS0CmvQs8Q8RQ1canMpvREXmE668wjYWDyAY6pgEsav7ObyU0y3kXlRpYxVxSaLb3ndwqDHdfjoJ9BwPuQC49rtFVCjZH9bN3f8r9U4QqRioEM%2BY0Fy5Fshpw1a9f3kCYB2ubFO5t7Xp6L4vr5mJP5yMS4cGqzyS85aBZIA9NC9G%2FiZBZpyklt2wcm%2BeTpgbWh1NKYFblWlQpKbLERuesVWxXxVIvbpwIXrkhmUxDdsuXAR3m5VfRRUQGr1DXD0WiV%2Bkg&X-Amz-Signature=1332ffb59fc223736009c542698e892365738458267ca11217ba77147c9456a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


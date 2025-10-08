---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WSBZ6E2%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIEny1jsBG87kOYSO3GSyvY1ce1CiS94IWwMwy2u7pMyDAiEA4jDAMOrT6zcKCz3h5TdAiReBD8ZSAS4qIGPgVLsvk4EqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOm8PqUZnc7uXMQYbSrcAw3SmhzpdcO4rNH1wv%2Fz0x1yYiJ%2FRm1x9SyToQplO4rHq8spAxh%2FGyTcrfrPZPZRa9d9qYKkC8e8TuXVmZeThM7lZhwSg%2B9YN3HVo2JUg16wGu%2Fv1K%2F70hIrm6XAfDBTB0PfxWexRT1YcsNTdfpK5tB9IFs%2B82xSYTbGnbj3ILVZ4%2FyASchQ08q0UOLRErKZTYps6TvfI5YZXAU2GUlX833vdck5A9xdvGJ4KOIMga8JW31NB7WcJIFnMj%2FujRtQVaaX6QWmxnhr1jwXnDqjp%2BD4R6m4vhbClW%2F5qv318aFjgytai5q9vKBbTP7snjEouYNPPQEw8sbz9hD1HIuqcKA1t88CccTwleChirNrpp0W30pti2yuVGtr43h8SjbdQg2fwe0Jj453jeYXkNt8x4bxc%2FijUGAEOelzbKTns5BDVC9CfobU5Mu%2FhXvzWNT7u6wXYeBEImHRlUfTe0NuAeNWthmEVsLtSXjsc98%2BqHeu1OxTybBQjktuf7H%2BeHLdEzsy7OBGQCTt5oby4YNcD9KPAc2yQZn5DZC2RSbLlNSfQE4wB9niu8gx7%2BnWHwE6QNF7fkNe7bqO0N6nWI5sfwBF4%2F4zZqOkTJ2jTtBzAx15MnIYw2phncwtvozGMKPOl8cGOqUB3Hp%2BXTmv2ux1%2FJHs1YDdSEBI1f%2FHr8ti9RZf%2FcEIAeGnAE119l1Ar1Irku2Ww2NRPoHR8HWc6KCNErXaEVJD9cQ%2BvxxEFiZrg5mpCODSwa9IS8wjo0D37tc9iLdkLEnGI3K5Dz9IMFp%2FbTM9LHiMQ6HM9CP%2FmqMlkwHSCU%2B%2FTBSq9WmUybmWRP5uuH2Zbfpj1w4v6Z0YcE73QGsnDU26OghUMRJf&X-Amz-Signature=c0afb68f343efd8fa65f8ca10c59c411fc96af3c87b01ade408d7f6697da357b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


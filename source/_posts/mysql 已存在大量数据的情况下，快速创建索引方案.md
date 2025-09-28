---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQKZFBQI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T100037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIH4iK70Vkykl83iETkgdZ4uXC%2B5FUYryGgeCrljAmVOmAiEA8BtOKrt9CgwveOefzbteHajwiKVDb%2FMEDlRtJBq%2FEFQqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKCr7d%2B2p6Fo%2FFPc%2FCrcA9KTR%2B8KtS0uJuEGDOXE%2FV5Fn0lIcmsaO9d6RHLbFffx0GOMM4MqnfxK%2BrHyg37KZBoy5ro3ZyK%2BARqam%2BSIb%2FrhDcdMAfYoRt9edQ3nhO2hGZQbC2JIkKe7QspGpMoJEU5wY3SMjGU0EZgLKOOtUC3lnjovTTs3cJIN0kOnrMv1uitEDsAjxED8gxpcOqUdf6U%2FILVAds3bOh5EcmBkyy5BWPA7yU4jJy5CPjXh10w9zJ8BZPj4dHio73fd%2BRupgxETcxMR%2BXVt9NyMgBWN2xeo7Rm4HwVxKMr5EhXnrPUG4g25xxrm7bkYmOWAnQNR8OYTbHYFPsED979%2FoYoinQELTFNwfgY6aS2cDbUVFPeSeEc6OGGJ3DyivtLqOYwdhEoJEk3lIlv3HAFrJeaA1wc4OjABzzuH1IMDG8sS9wOzXF16rBWtxdTAktoQtHnrAE2do8NvaUEO8KYJGdFDcjmmLkSMCRD7NNrHyhRdO2RO%2BzgClpKsnPqQYrh6x4I8CwCfebXgDQ%2B4D8xARPt9wOBTLuwg2vFGFyMb1UosY%2Bm4TwXLAGjrTF6xDBRsFKCLfBz0GhlgvgO3sat68%2FbQART%2FpQ0lXwdvTiHWoe1kXC2T4Sozzj4COC%2BkpOWPMLa648YGOqUBJQD7wVeIQ2hmKjddm5KIqyf2v4JXkZ8%2BbfB7nqdhH5Z%2B0n5w0gV3ku%2BUNOzebFuIY0veapNQPzK6sV7TFQz0IX5ThB%2B1BOawnm%2BL6TdS7eD0uCCxy2orUDZSKIzX7ewROwOBEotHAMXvtE6Wx0CwFtG%2BZdKst1QdVGXAD8d4%2FblLxxE3BDAPqhnHuAzhR1Z1AIwH%2Fsj7C%2BPTjEQnNn78cm85rPU%2B&X-Amz-Signature=3e648326b90c5fe1a9cddab9d792eca7ac069257053545ced5dd9a79d11eb28b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


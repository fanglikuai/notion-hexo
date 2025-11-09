---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LOI337P%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC%2FEWawZekO4cSy14Xfl0fdnGO%2B5nHLGKYbFWtNYKVs3AIgWEALIjHN9m96DDKlCSa8r4IWNNPWp%2Bqyf5BPtZIpTggqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLYbzgdCCxkUSnKQQSrcA%2FtofZSPGcIYe9HgoUvk9EQ3A%2FMP2%2B95YCkpOrlCFB71PCPC8dgb0VAE65invOibAVRUmTDBNADYzq5H7yPFx23QE2w4LzkeSrcPTJ%2FLA%2B9nyUuHvk3i4k7CTAiksiisnzdZfJt9IMcCzGPBwivFkGpZP1ms4jeAiKSXHOR86IkQKvaS5MFoj%2FFbqVMhmSjkX%2B1y%2BamOA4oqrNQGwjgLZDlfPIsQIg9CJ7IHk%2F8jslBumVt15%2B79gGxyRy%2BnqXOJYQ3TYIOeffjJV%2BeP1DSkMHMriyUb9iJ6svP1rSbMViHGqKwbbFDOKQXGBygAZz53ZbxMZ5Z1MTMz2%2FfQ3UhSpsC3mjxYitR3BxUzHTXmGsdFHTCdtiCpvWqlyxnxk7LEi4cboesoAXxblhoOQxAX7libEnAb9ap0%2FMw8rSLJzIhSvGfaRTnkWSRiE7Sww1AL%2B5tFP3C5nqqRNkSy7Obmb602xlBczd4sv8WYogaHup5vKun1Z6SQH6WwUF8tDKxhHygieqD8WVuv%2FSKjUmFeLtR8hksOIt3z6NQaGkFEsOXSFiNBUOZN6WK%2FejqU%2FaqCwGq%2FlgQjmWi%2FAGWpBFoS04umc0rzYty2dcijr0%2BLrthnM8zP6G6pddHaqBJUMIDuv8gGOqUBsuYFmSqsPaflKCQ1A6fd4OT1D7xVgQtYuL8apPRqOYIJMZYNrcjOB8HAGJocegsNcETTYjglwgdOHvcl42I5PdyipOCgV0gUOf%2FOKc6Yu%2BguRsSU6W%2Bvs1Ttb1eS%2F%2Bbla3QTiX2xReto%2BSzhJVdfX2eTKo0fvQ8QO%2B%2BknbgzQdQ4WQH5Xh6zSXIqtf9Q8Pb8Eky1Lmrv0UMCePghxKBY9IaS5R7A&X-Amz-Signature=865d40a3171fb02dcc23f23d2d0bfb0dc5674ffd51a4b303a08558d43da3b074&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


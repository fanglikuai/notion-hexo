---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB7WXFJE%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHyVquGJuqat2HRFOlxKK6ufWq%2FjmVOcCAh7QuvsiT2%2FAiBYbkbqkgMuzqPytFLSkjzAaE3tcu5nal9GbnzbecpjoCqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcT4YoR%2F9FuM5iqOSKtwDAL%2Bjc%2Bn8I3JKrtlEB6JafKXImKKWt8VH2AxfjnZ2SdHEie7FEPyr74HjDhYSwDs%2FD2kPjGvYamdCcDJCnjlHIdeHj1w91cZm5mTcv1ZtiOLflSksxgpW7aBudeQ1X%2B8potMZt94o0KfDDwryWUSRvCRV1C0m43c5wPXE3BCzsRvI7up4D3kemrin6Y8qdsvZtlRRxzTAWz00UCeKwtwOFokqLaUJqyNo2KAtMG%2FLGQb%2FhxPRwC7ryLiHAsAVpiGFT6SgkO%2Fudda%2F8vpBXa5PErf0zLd4TMy%2BWV%2BJtFihy07o2qtTwOd0TXay%2FJmCdAHoKX58k32ALUickNESxVMNgfj9SFYMHE%2BOVlbPMc0gnwRSjCwH4yyz0ngHJrsw775xPhB%2BnsLB30K4ERnNszjBK%2FMC3dthsNX92LAW%2F1MHyaqMi5t%2FeZb1ffHxByyytC6h5jAOokRSV4TQgSBPvF8jPUPogbWtAgoGG24JqeLgarUZE0dEwiqwJ4Nj6svjsXERHM2%2Fl%2FqutNsBAxCy7sstYNJpEoUChDP19NspeWpn%2FzlW6Mvn6CwthLc10l5G%2BsZVW7ev8Sd5k97LhrwORNcgjK3KH43It5Wi1r1qPL8g6vp%2B8qnut9IBGCsOeX0wpe6syAY6pgEiz4dedaiKa5yE5YbuYQ3ncxMkWZIzl%2BWkGh%2BLiOA0a6HvjrJJ0oLYRjJalUYKNhFTN8lYVaPKfXOtwtU3x%2B2kjA0Pg6VgRsNkFHzH59T3xhEurdG%2B9IZfL5N0mHbeIapB89PnrWC6YrWzgjqny2u3GNGvKvKk5x1T7EEOe29nGmGe8RxgjEejJtNvYNWyDmfRuao09acbulvSbuQVjisgZmDDgub2&X-Amz-Signature=a55bac32b679b7c4f062d20a61afaa5b82d479e0feb4f135ed0191b658104175&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


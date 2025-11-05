---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VOLOQYX%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCwA9D2xQn4Bbktly1eUcQqgnGwvW%2BBnqu2VWzfHvhARgIhAJd8WQ1Oon%2F4uA7tAXRRspw1kDeu41CeLnWaafPJf5cCKogECIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBfxRxV6BdLFjJjuoq3APG9YLhirfWbcWlYoALI%2FjhCOz%2FdYOe2feHS4Pn2v91B9EDNstkeUgE0JpC99sgWWz6CsplXsvBH%2FC7zEew%2FFNAtu2OGICb4l%2F5HgUhC%2FgcjnujAuXmzifbW3QBdHGl0XvHb5C%2Fk%2B40GYa1bhyq2NA60voK92UIHXsIrHWHW3DtktMRZJI9mKxjrrYEDV%2F8PXkemC503qtU%2B9jecPPgD9vC%2B3qru3vdGVYmSuIOvR4VxCpX%2F3JGstBQyqKBrTI8Qc0mJijJ6PceglnLmi6tTwBgM0wpwTslKQA6uSo8Cp2oH%2FsLsSWmbqNu4%2Bar0TBmm3RsAZaXaVFee9lUQhtpGLbt0U0LvKoeLNo0qtGm4M6ApjYsmadzwPvVpGbv%2FIDw%2B3jPF5dCOGJWR8eeozRTVtF73RrnUdYcmoCA%2FqwkyA4BJ7ToPQKdUt7rmEksxA%2Fox8u2UPP4nje7zU408EzQa4CAHiDwGeitqTE1fJfio14zwk%2B6UQa3%2FCDMbgBhsTfY18oY5yBgiY3PMmiL4QFswnhPZZ7Dl9k%2FKmfF6%2BF1zLKMUf8Lt1rASUYNUoJur61FpEFM44swBVlV%2Bxs8Cz6Ilzw1YT7hq6ABE%2B4wKK9MeMvR8uETc%2FAj4iVhRCCCMDDSoKvIBjqkAQeHOKDsN09alF%2FHAFuZYLN8uWRNHzFxm0nqmPKH47ePhJZVBn37nsjzev1C8ypYiEgaRGvzw3V55RoztR%2FZ0G%2B8hw8bJe8MZwXu%2F%2FwZqBL%2FP06C2%2BFJ6gf3W%2BOCQtEdIVQvf1LvI%2FAVMXQ9urLk94mL6ZhX0wcYCGTaG0vp57tNln1Lh1e%2Fb4VQ%2B%2FaIrHIa5UuPufOltKwWkviasdLGov0aU4bV&X-Amz-Signature=af94cab69ff283cf328d86d109bc9bf13532aa6f3a0a46c52b567d2e66438f89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


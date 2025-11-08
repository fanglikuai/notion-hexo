---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE5U4AD4%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIGP5qHl7zNGVoU3KgmayMbDJxdFscBHHuGcifIY9%2F2NZAiEA6LgYBPVajtxCOijBe1NtcMbf0vIZ4Bwp3YX1cLB67fEqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOd2fafRwhYbiVjTmCrcA8VVB8eHaqj3EmeDIARIZXUuyGd9%2Fct1C%2BokcG2qbqNkZmkHIN8eeZj%2Btv3eztANYfnTl8Z%2F8%2Bz5RBCxITw04vVyDweR4t1TJQk8KCvTUk8nTlYAF9kzr6fMNAaE1eSp0wl7%2FZJ9SiGK%2FmEM6JOJFKBSYkwVJlAWGhe3m7jQhHbmdOxHdCfa0Wo8ZYgmm%2Fhu%2BoY4Q8KBPptRRIgDCdSEvq%2BljCxLSTuACOAiPUwNUTdGO9i4i3OvDlBQ%2FGJ9bYvzLmB4D8bnSq4i0pI5UwBsddPjAs7gF%2F4pTA5RDI3Z0VZqsXAoyE%2FCnHugh%2FlMoYzj336sEIpSGcxgTPFXxwtukoIkfQ%2BOaobdu5z1xI9fG%2Bag%2Bfs3i8k4HMX1d18LUSfoJ6bP1CkG0c72rWwgDCOaL3zQrzRTVZ5XsING%2Bov6Gdy3yBIVuOVVqTZph0iKLElOjBkLqeAP49dbj4LAm76Y1vEC201Ot6jUHBZVvxn62hWf1SZZyQyZ5SM%2BduRhA6A84QbCa5mTJFRBozzcbpuegvLTwE7AsBg%2B7WxptnerjERrpuPVXC04OvOL0EiYp2CNczK9qlxQvIaeRlxLswxblwAKNzvpkBxoKg5VTm7ecjnHzIhePVvkuzGe3aI3MI3ZusgGOqUBqTlAceQKaNCP2%2FDakTTYid3%2BmBaP1kPcr4ihDxx%2BZoGR89izMywpJCw%2FCVTpbYkV47vUH0x6gHP%2BJ%2FuTUdy0PAracMVjcDR5IuogZQ3xotGi5AsZFQcbrz2XkWURarWOLFDcH1gnKcCuYyphxe0GHvhJT7XtYT2YOf4cF8B%2BgpJUBe6V4RXeuGn7nTsHRkwDGP9C399TgWoSGjU%2Fg6C2Nf4qte3N&X-Amz-Signature=719fdf02e7f540a0ec7271ba37e71f1bab57677c328d1ea72843028d816e9bd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


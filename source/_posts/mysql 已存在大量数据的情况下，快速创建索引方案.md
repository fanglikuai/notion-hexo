---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EEUS7XV%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCzJ450AQHClT6dpcApp7U6QDHdDj8IoU9xd63BtY0pnwIgUBfCLZHKB0pjYUfmzbapgsQkEBsyAjbs2Q2uN%2BxhXqkq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLpNpRu%2BJLeMn7CiZSrcA%2FFGnQgA4%2FtTuhCTmaVW9qPSNsfRWs61dJNdnIUoV0pc0AOD0U7bupiLNo115URzZ%2FclZdgV4g92iGxjFQ7r6jEHynz5dV35iumHWxh0bTTuEeGWluWVO4SQzaUHXve0gqt%2Boajzfh2Ykl94r2nB2CkeOyPpF%2FLzGm3mo4DFzO9bo767EXV6y%2Bd5uzJDFMFsKfdRkp%2BZYVXsVM%2FwutZ%2BurmOEWLYOs3Zlvgrb4T1E2h%2BhFqE8TTYwkQHW4B5iiP%2FbosGtqoIbwC2os6imAoQL1HcJEUceEVKpxsmcaFpffHO3QkLJ3FM2U90MvKgMDII%2BbqfkH8jmsru45GjtmH8T8V%2FZzsarvhx21DV6T6NO%2FXNeqPAXDpbkb%2FykcjXs44teFoIE9aBE6IuuPGF4DtNaSDIrXQuGxECzOyDUIVm3G5gd7ZlnKhLjFwzNKUnKDQgvy7QUJhbFAHlXOnkH2DKVVfT%2FVdhTDO8lxbj7oVI%2F%2BQZ2h43jfpyHWuHak7ibVrEG09Sn3db758dNpx259xBpQ%2FOiWT%2BVTmBah359gN3GVvPDOSYW6L278ZfuPnh7Yd9YYfj06uAHeRbmKyyQ1qbj%2B%2FJYwgyjj5MaTeT6nsCwv2EQ7fmcY4O6vNEBmO0MK2cv8YGOqUBDDpNtfSad13RLNrorvhNU%2Fv8mMuRv1OOjmQNDxJMm0oSMNF5rzeeIZoj8F%2FuP%2F61qHJvUkSTqCxQZisWYIk57v5OuwlqUgna2xkGyzQtXgKX4K2ktDfpJ1x0edvDscaWW%2FPRWhA968BoYqfHlZ9uqzTy6k3hVSGQRjtd65TEkYo8M8nqvoyIwzrLZAJ6Ov850V91pdud6Ee7cPp9cXXbfzcUBZw%2B&X-Amz-Signature=dddf8ac0e5e0a9261a278197ea08c861825beb652bd7c76d5710a2047bef43d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFFSJLBY%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGatPj0rTfbJdQZhWR7i05WApef6o4cpT9oHEJnggpoSAiAkcFwOMc1HHozeykrLHkXxfN9BFM9YKcTClsd7r7D%2FuyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMoru4WSR%2BtdQGHAjsKtwDCsakW6hgncYVmP96ffT%2Bub2Okmvdw7eBcp%2B%2FYc5UnkrbTpRPm0oZBjKBxWcV2sK6VDM6AaL5d15suMBZxrOLeJIdeluyKLWuJMHM%2BsTtsoJensV3sSDRgD5f4uciyfol%2BxANKHTAGh6m%2B1po1PnSxB%2Fgc10kV5pZfgAPFGjqSButJAOQut6jnWTBGoqeG%2FbNKj6iXIZ2M%2F%2Bkcx9YHmoEx9y1CQxSMSutRYeHxYUJmhSmOVbLZnPLZxNgU4DXU0Ws%2FK%2FocXS0ZF%2FsKD9XG1IBDpW8iJR%2BEE6exjJsRft51fwaY7AovrXafbEQ9sYnpe4DeLm4KvHywIGhMRZ0F7ipD%2Bnv%2Bq%2FWa4pLe6duPQi59GM9W4yMESHzVMwg8v725PTjkq79a%2BnLS0C1DnOpZW0l%2Bq3t1UTHDQ33crdnHX6TnoxRfeR%2F3%2F0YpGw9muIs6lQYssRgUtm99v2uZP7n6VBRNg3yUOEUarufLm77CCZAuZM9i85ErXKdoAxu%2FMRx1WqQcl2GYWXRqzl381G8yvnqyfZN9%2F43gAF3uECZI4JA5qKFT0V%2BFs2aue2AfsYiovfAKsHko6v6zWpQTWddcKruzaYA1VH1RrcmrjetLmMEhlShWz00IhxTqgVJIfUwsIq4yAY6pgHrhpvJQ%2FDncOTzsKcmV18Wq73gOzwKc0ju6YA%2BhccyYZ5EaDbshIUnIblqe%2Bx%2B%2Frz5cb63LcWBipNHi4EaN4HWlnRhOtNzQsxCQmR97qxx5edAsNu%2FyAJP9v8UxewU5jL%2BGa1MhRb%2F6RS8e5GVBN54RfXVt%2FwQsh3pHiZN1PgOn4ACNcKzZVC9fKMIPKigL%2BoUHhFnHLU%2BHDlszjn22UPKtZ8HjlQk&X-Amz-Signature=2c89ad5064ed20ab2347fa85488cfba067fb358040268aa6daa6e056f0eb786d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


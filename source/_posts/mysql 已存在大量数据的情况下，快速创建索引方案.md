---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FKH57VL%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIAMjC4bpjl%2ByrgR0fsVmWgy%2BxBvM4QrUUqdthRsHjR9VAiEA0ZPSdZZ%2BeZPRwHYPLtSOatebEhv8et%2BeHZx4CTB%2FWnoqiAQIjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNYCFVWxbx%2Fkwa9RUCrcA%2B%2ByzQUsWOld67fkhxRt5H4s8Vy%2F0b8FrNz%2FmGa2qEfeLJ9t2yzUMBhssbDbvXqkyFlKciuwTBPt8u4QSh%2FHnvZjqbfCsrUZ3vk5QCHF6dl1Vu6k%2B9xJW%2B%2BA9GGTumXO15DLC3Rgu0Va3kp4OacJVAp558vR9MNrE2q0pcTHoNmUwajfJkDYm04CNB2rz%2FZCNZH%2BW2lnP1HrP09tisKczyu1ewhEutGA7W0eeLW1dBwDrnOP4mKFvdpRMyooY%2FZCSBRjO7YDEb3ht8XQHgPuvcfXlKVFGitHJAoQymOhWkw%2B%2BNgdxy1AKsLIUeNELEHN8ee8KZp8GrfHoFtVhCM2hlcNfjNMpn6GjaPgeeA7WY6sTVaqOOxRscTq6RVnav6V%2BbubSCIL0bp7%2FR42EUBCnwNgqzwL0XdV0WfZivT0AciHHOPRCVD0IlmsolHv14ZFDAHB%2BL1VZPvloIsSzE%2BUtpGOfFpg%2Fa0HMxrN11xMlevefGdiNrKRbm3f2sblPzMbO%2B0TqDU83wm4VYQK%2BYEmGemTZPSFT21%2BWXVrbR%2BPHTj5J2nwK78qB3up%2BVE0dL8QJgoZigxAxHzCyMHqmea9%2FcJc82ncSUEbnaq3WCFpBfvP7N8%2BJb9hLjpYR%2ByXMIr52cYGOqUBPPClQy0Bti7iyMqVmvNRXqlSph%2BSMxXynZy1HjOqCVb1RDtvwYF1NruPB4josjpv5zfJFZ3TrPwlgMTEulFz%2F8MaA2e%2Feu8de%2B98%2FHNoiegNg9t8NH1LX%2B26pWUkH8Jnx%2FleUBHuAJn87lgqdC5ScefOgwMAFaLdMteNODqMIB%2Bq8wdmNwsJf1lWGm1lVObb9LKx7w7KZy64IKv0q4cpOx1EDWrW&X-Amz-Signature=434f45717225e442b6dae657a5d42aabd4ff4e06205feb7a8d5aae3fa4aed622&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


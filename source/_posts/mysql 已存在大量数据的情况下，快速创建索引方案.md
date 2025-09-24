---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SMRTDH6K%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqZw6KZEmtNkx0xVFzYpm1MCyN8D0DjO%2F%2FmU7JBcUUaAIgeQfmJrC4SKR9raoFWaaQ0SuLg2ELAm7xBtpKWTNMn7kq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDPUiASJQp1OXz53w3CrcA38DODqdcPZq1HO3sPmdfjHiUpykWTH5MgWvhvp8qRavTukc7EEcVFn7bClUS7K4Nd42uBMfn%2BqE1ABdht%2FlHnyZ0n%2FJUZggnPU8hcbgK2%2B%2Fmb%2FyPHJMmoZDvbwUF0JVXwm%2F7JrNRmHxkOEyAJ2Bt%2BE58criEbryoeeJrcitewLGjvqiII9f5K1irmX%2FzqzLQzuOHb9NznV8ygjQ2CL%2FCGhQNyqEg1M929vYN37SMkupB6keYDyBCN7DJlTMOPTqdkPRdvwovJAP%2FOYhtNwI7Ayj39UEOc6JuG6FJu%2Fvc7%2FVblcdDbAq6%2Fop3QP9azy7vTOwZAdapBOFJZxj%2FCQU%2BWLl2ah%2BTsXCVF0KfMmI1DtgnVRqSN5h5K6bz516KZhjTi78AYx991mVlSO%2FTkplHaVSksVBnf%2Bqnq%2BSHbFW4lcI4J2beGpTxTeCOdlKHH%2FokJrwBJN93%2FKemhEJuEWxmdzHLFYOjV%2FQ0H8M2sPOyI2RVq9N%2Bpg56KR7Nat4mWEpMLGxFYtCFO7kB%2FcN7PGJpCkbkOc5cQusvOHSoMel4JEkZKEWzrjCAqiTMvT3jHm6Sp6TJLWHOLxYbvbX3%2F%2FQj2AVIDd%2Fn6pKuzQoRCjDzgR%2F9xuinjTcs4R24iVHMPSX0MYGOqUBstakHI1O5htNayXl38vc%2FZwAUrsOeLjFFufwoYo%2FtfzLGa5aIti442ZmVF3gZ7kjC8l0fJyY9cv%2Fg03u9zKY6mv1TJdrcXwN7rSVBAzS6DtC2UAah5do81l7D%2FhhW4hJehTu3SJEnMJ%2FNSpJvt0xYTDukLoy4MMBU2AmAiaxQk57q%2BGWf13y4P75RHCMK%2B2%2FqnzMcKySHGtqUyBCyRb%2FguYuU9Md&X-Amz-Signature=580c326013cf856503178ab1825b469aa28a6eab6f648ad99d0ac2e2eaed06b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YABPNZSF%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDJkYWLuvb53uIESQhbhE55w6sVYxoIH1QjWHaNqTcYzgIgSWhi58a9bSUMnbVWJdE1vfHMRq33k%2B3NgUW8XPdLKKgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMASSMhTkldupvZsQCrcA2UjQvKwI3bbCiv%2B49djzJmdipMcbMImvkFF%2FZS6MQs0e9PHhHE3r3Z9tnFYU0qfA9QwiZQQ4MelZJKl%2B4%2Bw1z0LMVoICt%2FChaqr%2FyR8i9hQtrmX6b89McF816i6eyU%2F2d6WGaTar1dUGSMDWSf8Cxjn9D%2BToAr4ZqJgLHsi107zlq9aQOuEblya91b1nP9wn5wMQababF9qDTPMsQNHNSyvOT0fZL%2BsoO28qCQ2s7FTO1rrbqrCjEMVNR7gJ8LR0Y9OonKZHRJ5vp0hMdLLG2TDsTmImHrQYkvEwDn8%2FjSmL6Se5nALD3O0IPC%2FIVfKBFQAMzQxq5C76%2B5NdAo3jE1DkYpp75CvddpP1HP0PjThODHuwQeL0R4HSwMXURhG1QHMaOUyIaMu3YywO9cslXBGZi7eywpxxNZZOPCTfKsfU0k1ECI6XA38saQgFA15YRlsDwP95KUFCm%2FcgO%2FP7U34SZK8LjihQsMooYlSuyfvvHIN66OqrddfMTdDtVBzN62b6T0gNIcIUxc3a5j3lGrVNSsJ8ga1kqdRojAvm7rNEQCnT08E%2F2mRTYlrsaHwHcSrrrqBpRoEfyCntVN75SKad9TMUlldeISi4kWAB%2BzrXsHZlCGtYlL4jP8GMKOThcgGOqUBmU%2BO5xt1ArCkh08hAaTAaK%2Bs94tf%2FlkXzG%2BSHcFSR4KMWm0%2B2beXYb0NOJerSgyc5xr0AoxPRCqgkfZd5zbeKMnpQqA02pwBNiLVNFgG5me0pSxgqWK7kQVLIPMjcFK%2F9C4M%2B9JcIlmgRKdI1eNvL0xEVeJLRnIQBbBYe%2BqlENdO8M7RKiv1G4H4oS1VK503s4quF%2FoWNb2dcIGe4zGfPdi1h5hF&X-Amz-Signature=90e84579d88609ca591c25ecdb3121eacbf01a034ff7317dac9fb34dd444b275&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


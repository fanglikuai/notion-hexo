---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZSGLFYE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQDmwfW0d%2BjI3dTYXgtECfDlNX%2Fk5sLDlO%2BuqY4TC0WkmgIhAJ4IE3bnABWecfKKkyCc%2FzZRSKG%2BBpyWDYLjfltfTLW5Kv8DCBYQABoMNjM3NDIzMTgzODA1IgxObz8JREjfLsKGSDgq3AMHxTQukSlRLbdsXZUP645Z3v9vHnJw53tbf%2BUyVd9LGxvZEgqxe53NvrsjOElkTmaZYrN5W6RUonLEfYkU8lxz5qLJrAEVvX4hKzBGZGyH1xmRPUXybxc6yYy%2B7WFiA%2BkBNI4Epp%2B7jNgtgcI83vE6n386VUcBc8zI2kO3n%2Fa%2BnvfIm%2BBxdTpCiOfBCaqFN6UpfZEgEC73f7ZNxqCuImsKQUX24jDBmG%2F4WEZX07HTjvT2kIjEAOEGwEO3Lm%2FFD9xfGviw2xNNwwkgIlIZ1RcFt2tXAqVYU5klHARgIbnl6m03UzX6b4n4nd5Yc2uwb5a9o3tuVO%2BBoIgBTyR4QDrobd0N%2BSi4IIKhoRQRYugqW0NnkOFbpmGGhXJw8VItFuO9oI%2BgiFcfkES1b2vG16M%2FM%2BhX6CLCPLXtYcVJHO7ADt9o92Wjbqe9O4u7y%2F5sH08zq22BQawBwR7rsszwY5bmGkVn6Zwm03MM0UxcTlkoBG4Iiraf4SmDf1jCBMZV88NXcBBPuQgriNB8FUk1B63dJLuM1Ago7xZbxw6ppEpZnxiZX%2F0JroUI7jNNmPEH%2BBA1x2gtPRFWvebbnofynGfJ6E8t1S7YkCpTUlNt4i%2FdSHe0Z085Ok61%2FPNkHTCN%2BN3HBjqkASIBIHiMwwJ2Y%2Bq02MvCo210LtldMvDklP5nu%2BDD2kgelBXwmwS76KXr2btOzk3E3Syvc%2BRWGVmeiTpzaoVQAJqP8z5p7NkqJvMx2tbkuPL%2Bf%2FAKjIqgLeSE8UJuX8toZuPWBzvUazHTffvI0gFrww507uRKFXqXdZYiTqzIr08imzJDvHbo%2BzGLjvCgKHCCrQiAOn9AcZxCgR40G6OyG%2Bf8FPS9&X-Amz-Signature=3e1baddbcfd261976a9692fd29e171b3d98b19c7d5c4815071a6b986773f926f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


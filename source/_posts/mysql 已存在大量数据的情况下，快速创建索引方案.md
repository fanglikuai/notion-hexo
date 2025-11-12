---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBPFUTVR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQDNdmYecUQImvRGwdFFV0HlC8XBECQ1oqT2RWGVhgjfVgIgcByKuM3l0Km3OsPW3U6jstAzXO2myQD4zgKG%2F8n4t5wq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDBVF0Qk163ZTcG8dQircAzzWvT%2BRA93nJDJbE4vMO3ScN2f2tJVpwq6zlW9C6wg1Q51ocpLXMf8bEb9NfvQUBiIlfe1eSxuCpx%2FhRRwMEBM4sV%2B3TbqSwByhNMzJ4PMd3py3IrPQm6uIOYXrxjMZRYdsxY%2FCmnT39Xcg1b6GEOOcTPAw8lD%2FZgTfFpPQC7CL082oknJbLIsnmU5Tw%2B%2F0028bCtdEJ6vQ30y%2BKj1UnX7NSGcsN8e3ShPinWtj9aq5t4rJm44XsfpY5fref%2FQgfEc%2B7xuHY9CDgdlx%2FG%2FVheNmgem4BitE7tJ13yCHmWKol9EFmnuTSWv74jeKpc0uA5pfRxxGocgexBAQs4W78JZcP8EsDwMLdzF3EuGKmVJlODRoTNUUOkxUvrQ8YZ01%2FmSkOVtG6sJYwUSvyeNSj7Y8hEljYY%2BxLcN84jjZ%2Fi3AgCpQ4tsO18aLIelCC5k4wNwYfhHoUZ%2F7q8xMtFqdtHdo4y2on3zqm%2BSTBipN6WGRszR7RdDyxDM0rOoO%2BMupTEwdIQ1sx0wSQLagMJ3TaEZd8GsqNK6rtQdSNOkw%2F1mCWj2vfWA0QDU3dG8iOFUhjVP%2Bdh0oSAl96Ww0VnGspjUy8LD%2Bu704JTJzBRd1AFZxg9tF4TxQ7vN9t%2FaBMMnMz8gGOqUBiISxLUjfprh1bsLOF9tQZm98jkKBQXTinoyhI2PM%2FlBn%2B72o1HURPTLJSIhA96q3vXOLPOyPyYaaAZI5LKV%2Bo4w0Rl31xV%2BvycdqnIQyj%2FaZPl%2F25qH48BGWML5B1w4tQRqfWSnm1lPdZweGaSqk8xgavjPQemFofqrH42AMIdS%2F%2FmuIw685xkMdIoLKD6o8nvnxjv0gCDXgyLy3sOiTeGSsatC2&X-Amz-Signature=617ade06c3f91c46b4293720656be4954052f8a09202fda8aaec2c6a2c0668e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


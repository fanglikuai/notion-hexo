---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WD5GFKYQ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIApZxBeL%2BCmAhPSaSL2DdCGNUWTey7HWK32LuJapa2b9AiBjbrlspTzqNZT7Xyo0NfvnESPtZ68HfH%2BQBpzQsTu5EyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0IMDIEu%2FN2GjFvfRKtwDK9OY24344QwKbVpxlkfLc0VoRcqiBbOJ74jeaPcYqfVGolcep89NiseVb%2F7rrr44Fp%2FQi3KG2LBrTlA8u9dknDzdfdyfTxQuZglZpP002lozKjX7QqLVSUnqhht9MmJP%2Bmhz4M5YgPtQXzE%2FmxVuaoXr%2FyUj5PzeyCz%2BkJWpvk5s0p3M5bx9sr0PAISHkfjQmBFgvUSB3HzV1P62dhw9uvmMptDpyRVrd0d%2F%2Bved5yxIFGLL2eVlqif9dCHhtq0mJEfBX2bbQtIAsoVSc2qLqwDJuN8Jq63RN%2BcwlYvONSrrX70aJl7Wc9xqgnWncDLOviDwwRY2A%2Fsr%2BCvKAtpAmV5ik%2B9F9OdFwnQivXOuPZDAokhRvU0%2F%2B8TujDYPVcxoO9LXsA%2BuTTO3%2FKT0wQfLdNmpHqsowkzb4hlGVk%2Fq7nKsHipvmKTverUJBwViRf8iG9c6ajTWUeOA1NZ%2BPOozeiVzZKJd1heWKuLeGFf45G4ABhWm8pNHsZycuLHimmlm%2FNR%2F%2BitLaImWxavP5q1s02PCEneI2xgtJg3usaQD%2BCQfXov5nPGR40tXrko%2F%2FAV9BNnJz3MwyycfUK19i%2BmvbwH%2Fl4Cmd%2F%2F7XeyoTq65Sfjfj6D9ky8BoK6NYVMw2qvnxgY6pgGeY9MZowIVvSxgZ7HmDLnGETXvgZF3GwwDrGZWNNMkEbeW1xefL9ueMQXkkZHkJtg0CJoS8qKLzqihH%2BOFr6I1tEEUBM65uSyk1eYvqwmFneRBdnpTVQfY2hce2ynewGCCCUEtzSaRhHz23nGn8rhBnRHZHgmz8a0%2BeyNsk0WD72VGmVDrtLy%2Blj8JY5WpwYrEqth3LCTpWVLO2eA%2FkY5unmAiUog5&X-Amz-Signature=f30cf3ad019c3cd60bafaf87d72c7e6e85b8b6e82cb82c2d27edfb277494ad45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


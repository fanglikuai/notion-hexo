---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656JPYED4%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T160038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIG8wQLORvcPHJ4a8V8kHUFKGo0H4o0BjEAlW%2FA%2F6J5dPAiEArTSw%2F%2FjImlGfifxi53D68GYJ0q1XJO3NzR07Zg5N%2FJcqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM98q93%2BUgevQ5ELpSrcA%2FpBTK1PJ6zoo9ybnnyL7jT5qwyNDKDAnEKVMLeVPOLwgYnjy1WBpLg%2BwKmpFPBj%2ByTd25OE6RtCtgRS0E7%2FPpyRWvB%2BFHhwL%2Bq8uhZGS%2FPR1oysEHoagPWnail33U56Jswx7NfDdKIUDuvoDvt%2BLQm1AuvYlfZNyocmglV0nsIyvxkEtGpg8jsia0T8u%2BsHwhisQH18t7k8Ai49BOdVni3J7Lgima3LXK4Oyrme2qKUFVUiIfusjG4eE0akJ2vDsGAsh0LAraEloWRUbhSc2iT%2FWaKmTLye48PHyqbl38AXIIDK4LFTzkCaz%2BiXn7JEPucaQy8qvV7JsgfVuzY%2Fz4g9ZkguD5B8RdkS73lB1YUDzFgPRlS8Wj2OxCTDd4okvhv6%2FXrWHDmC2Jmij4zF%2BcfcoCno%2BQN6xDH8YdNRla8d6kz5ajkvwz5i%2BU6uiQoH5Bm2YUyBZ9iBxRkrLkRvuHlJpGDTQD%2FZRza0A5FDXbInQs1Q9gKT1gnrM%2BtaO2VeUF5V4SHx23sG9W6dAY%2BbS%2F5mzsYKkx8ZTn2bgYMW1VlSyfNkH9n3HqsxOWF7L%2BBAyBEDIQ4txlxQ%2FM1rg%2FUOBKuRlQn9TI7rAYoIhfObbfXZTdDXd6zDyS%2BSoCY0MN%2BKzscGOqUBqRog8Xb8rQ9M3Wk5PqgwR0UDE0%2BDrFT%2FjSzT4msMsmVo962qKypTkgbs%2BDIJp6uZNIRadn8b4hMaKs0pFaHt34db0ufMpSO%2FSVNYeRNwKtoQ0ep7%2BZSd0BFZpre5tnWtY6ou27xzPMVBTlMVXg%2Fhh%2BPHSOPEGVQHcGHWeZ4woNTVpMXItxlEi09n8VfejJiWFpgTQnEYPrzJqOMqUf0oVHg%2FJ822&X-Amz-Signature=cda82e6b602886e180183071d5fee3f3411f09ed386de0a0d771b7986d3c21aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VLVLPKB%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTGmtGSrPKvJmnEnK4dPHi%2Bo0jRI3APRa9May6kcgNLQIhAKwmt7AJb3MQjN1m1WisteKRhIQ%2FZ83gxmXIg2w0OUMyKv8DCE8QABoMNjM3NDIzMTgzODA1IgyDmY44goKdN0K9guwq3APb5tO4TlSD8uUwJKABhbh0t4LbKrFJkhcJBKkMrRlzBVrC6FVqUlMVdfttOs%2FsH4EsuAkvapev4GBjS0VkG1rbMJ%2FVaPalWRNWjCD%2BANpCJz3jnMqLOcpYhPy5VFizm%2FpYraAeT%2BP1gNXBKsLY9%2B4arli%2Fk1ijPr9thuCyEtISEqe21jAdPPPPjEb6BVp5Ovw9zRTnuIvID1XUcXOwawe%2BfienDTf%2BfXyUIRz08TFRwzeiuQV%2FHfq%2Fx9ywsjLaLzSN2XqBM1ChTEZ1piIdGW3EvvcrOdZKt21aroeEELeNGUIdmDQjcQhp1U%2F3TmzkBNsPDkM9wCmsS4n54Nk8iRFjbPuwhNXHw7We3AtsTwkZ9tVqBUGT0u7zNLCBlMSsnqkR%2F3znKAflmaYIkzor0Kns2puzo%2BqAs8XgTnCgIWk0je%2FrxwYDLJfJMNrSoI7df1rcwm9GAbVB%2BKnDbySUCJKYiAQtlsvqY3B27F4gU5yX1w%2F5sjt6rYsfl%2BPQBBGumuLnxts%2BHihGHQP15zsdInRb92JATztRRO8TAYepg3d0wI%2F1%2FU8qWo3bxPWHxM2NKlFZXu7gaZRXN6vbvHMDc3A8D%2BPqCambLFsgB2%2BZS%2FTwpUzkEZ38LYD4dMlTaDC%2Fm5%2FIBjqkAW9d6c2vygn31fNdnBY6I9gL3iQeSLp3otyfRoGwkipsx%2FdUO5KZqIBulvKJ%2BKGhp1UH5liN1bbzN06Vc1VErB0v3rSzo1KYvzww407Ik5rVbfmtRiKN4eZsB2pqif1lriJYJi%2BC7AuzveHnoIoRUCDnwPhChyDglv6%2FUw1bnReGB2WwuB3WktdmAzWBFFEfdtgCubd12R3AJsUOM5NmjDF43Iz%2F&X-Amz-Signature=88f98a0d5d07da6348472fa76c424a4a2d2e1cab3fd65e348ad4da8fcec34543&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


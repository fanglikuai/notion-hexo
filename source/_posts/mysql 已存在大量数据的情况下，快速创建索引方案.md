---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXESOIZM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T180230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbrQSYlZE6%2F7rE%2Bj08JG4WRnd6Sv22NxxxcUtAknx5DAIhAIKDD7EQhtoKcCGDQWI6W6clO0%2F%2BVQc1ga4SS02hdOFFKv8DCFsQABoMNjM3NDIzMTgzODA1Igz3luFQ9LdudEi8pJIq3AMjSp7v0tk4%2Fe0onE9NpHSXPnpkWbIHLZs44g6Mu2M%2BCfq6SF%2B3LRT625JGdVRATTC6Mo%2B5%2FNUtBMA%2FScGIAHbjne7tuAHWYvQ9luuyECEpDMlNFIkQBocyqgswENSw0tkFOKwWpspnTu186%2FXlla8KSZbyt9z4QVbYZI8QVVq6ZtFZmytZPLBdpfx25VGnMgJtR4LlSiLQfC89tad1iVPdfsU7PTLCoa0yq3Rl1ueer7KyIp2fIyBuTDlIhFt3RGAhMNC8un1W9slQUnmPrYzvwyoPyqhmn51o8UCBknRjHaow5UQqbzN6tm59Prd6rWRnH1SJCavH1FzqHkMO2czpUCmf7I%2BTUlffO8bD2y2ZZbU65MaNx5yQU7ev2cuufIBVbBWCB9rBST%2BqA9ZZJSuJbVk13SJ%2F7EuUuurIrpQ%2BKBDqPg5PUg413lqi6XMyur93yyx87v65IdxY2PUlP%2BpJwIgF6bNprTn%2BC6rY19gwS%2BHK%2BRc5v60r8RGCa9S3Ut058dJzH1ctrgPmfN4boslenaTXeuLmBw%2BzKY4Bqj4O4VluHf%2BaGReQiCfu7YKPUewxpv2ts2fKEnjtA8PkzqFINEnHYGZ9amIsRdYNj%2BDa4vbSR87aRh%2Bu8yj%2BHjCkupLJBjqkAf1zT7JuIYclEINugl14Kx%2FffjPf%2BojbmIH9V0o41BsoDzIkUJRrbdUmraSQD7geQpxY4VDT649AANrZZq7xjIiYi0xN6cZdRhfEoFzc%2FR34skChWycukljlwNTO2ltzxwfBENeniKFf%2FF8tPhNPR2N22CEcNM02ecyBeJ%2B7XnC%2BPwBi7tQ9hFUaXDCd9OyD42AoUTYY4ZgHUq%2FKe0kJM6fxhxzc&X-Amz-Signature=b450291410852e6dd9df7f69f08e232271909cd95725e2162028e38e24559874&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


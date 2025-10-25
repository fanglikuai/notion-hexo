---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WO2AS3C%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdpNCyZN0A7pQ1Ot7NX7Po2%2BLKJW9%2FE8TS3QCBa2%2BIOAiBy702cVrYmo4Ram4HpoAtdxHPW39fbNiLtaqHEVL4fgir%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMFLOjHE%2BVDbV0Uf9CKtwDL0E8wqLSryPSeUOuO73rV4yIlr3P%2BMAQsHudPBy6HnNSgalKccMxOJRQJaYfuhTmPeanVUI3e2GmijhTCpetRNL1EQA8cfH36P1I8vu17W6K5e5Jqqzska37xJ8W18bOBcxFLWE4LaI1eRZHcdQlWWr1uy6xABxXEp0xLg2tJWpN5QOsOT7xNQKw6mTFobTBU6IbBJo9NKAKDngzaRMDq9HWdg4O%2FabSyw%2BnNSOoyeTtVUJxMLsh%2BVa%2Fwz7l1BbkVIUo95xFtsoMnANCrwIOZcsI1%2BViTSjZnEqKWzApZWh5gxTmCCb%2FpOSjaI1D7FWyKLGJk2VLTFRVePiGipYOs5ioPkGUx0E%2BYe9WyT539cz%2BMx2EO2fw83huBwje1GU7KchJKnT4dW7rBtE9LU0weX3a7uew41d8shdGh4J3VfDj4au%2BnOTBm%2FJ1KJyhMeyWwcfBP3crkStigg%2FZV0ORwdFe7ebUlZuYHkkUvdpbwR7u25SvoG%2FeBYajRQ07iOIoXtXzNwQcfDrtmeOXRKJz8EEPw5JE6cz8wtJtND%2BIc5WfTCCfRVGFwx0sHzx3zOZzuV3FoyUoJnYvRKuz1F8day8HqnpcOPnXs2GZShCCd%2FE4Ls5dmH2dlORpRrQwtfbzxwY6pgHNbzbCXIFb%2Fg2S3%2BSMyUBy%2Fm03e1HT5JGoBztUetw6KlxpkmVQi%2FHz33XWhyUFqWMwEP2nVAMBdNnKO7Wt%2FaEMVc1Mlreu5UZSBQrOG8WZbCaPj5iqqbxSksnNmXpEBQmJEqK4ODeVnM7TNX6xz4ee0DJ9OOiJntciqJH3QO%2Fp574wpMf9QosVMDFvmVzwpA3IZWtIGxw%2FpAd4bU2wEXiafw%2FtdoR0&X-Amz-Signature=63e6d39b970cc291e9f5e195b8b45ce5e8d6e1d06a2d800f0eb2ecf49b185633&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


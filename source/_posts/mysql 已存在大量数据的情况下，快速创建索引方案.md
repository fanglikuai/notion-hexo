---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHIKXC6K%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHesIzcOt9Bxan09VcinH2Wna6w8%2Bkt4QHjpmKmo%2FFvAIgK5bdi3xrk9jsXLCUnQp5Vf4sB7Z7QsRDHm2WJaZk%2FAQq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDK4evqEvi1mzF9XvkCrcA%2BkrWG1XvTTBMdtymcaWPVnjCHgxqPtrH1nDuxzfojZnPbYO240eTTweHv8jereJNTYP6y6K668aiQkQ7fe6pj1lXWbfGs2GimnzuaEc5Ij2QNvzbhbjiXkE8d2gIhTJ6KC2N5mYGznRGkkB935skhsHgHBuf8SwRfWNr%2F%2F3gMzMYD96ozDViigpytsJkHNjINVc9PousgC69Uj7mkyCrP9EJNSypT8yTLVuozrxDfrpa8%2FgPSnKtiga2oMJX9oT03r7E4H8nLlhCon1yTCw%2FRmB9cUVmdGgbluR5fi17ChdlY%2FIpeULJ%2FemHQewSTPW8uWV45g6T6%2BwH2aJV%2FXZDP5DPxsxISLXvzyxDog74SjE7ePOexj0CULUoXmM6AE6nGGlTD7yUamawVPTZVv9oLdOs1cfvGtbyUCLyQvSl840FSO9oHf3kv9acjyz2nsnrh9BCxM6hzHaqobvdPHuBhHbgxnxGyP7uYli1pISSm9OaCE4X0iKywUXmYOGID01cJzspLcWPdvQh6LID4bIM%2F76ilEg5nmZX3AslJ9MrnegeqWvvonZ6k5E7JivuNg9WaB3x6mEBfijif%2B9MdTqqXnKyU8XPVr05P1Bd8gHFrmbRHCd9wK4sFs01HIwMIOMyMgGOqUBJ8bhI3jFdbLYHHe7p5sSSyP894Se7lBNJA4adBnx7qEre1YwGM%2Fq0rkoOPcVamtxCbttIf9FutvJKgyUlCj2PA46nMCz1e9awq2nHQsdh0zibHzhR4kTtk7u6Mu7UGH9Z1VOGpKqk4BEkDHWRTX%2Bbu7eH8f2vnIAcDQgBGJmS%2BFvWQpqU5q0uEpAe0IOKsL1WhfniukFUe5nEpB2BZpmYMYCtMF2&X-Amz-Signature=9c567d645b26a95a4ec107e576cd6b4f99f3a218c3754a966ccdcb89a60453e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZPUX4KG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T220054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNGtIIWIsKgqHH453edd3CxTBoJEAYvAgVoCryJfy%2FsAiBRO5G5j9u%2BPLAaV9I3zWJ6Dzc2%2FrZAKi07d6c9LG01QyqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMR%2FEKSPgE3v6zkqmOKtwD4Fy2pK%2BUC%2Fr8R1ErY1iIu%2F1P9fKe4TWs7UWeuuBEukUbmBJClDvvkHKsQHKmaG70Xr3USPrI7nEqUO6XjMO6GLG2g2qDd7%2FynKSlFQOcVwmFgme8Hq3wDo%2FYCAXfuaabn%2FHKuSw6Z5dJfeMyAoi6kzJtXO0OoGuc7tih2n5I8iOP1REbe6%2FsNPORLfUsj5wi%2BIMKIGI9HMOj%2BSuoFWwJlgxA8t9RsxrDKVozmrKJvQ2lE8mQEUC94Nf0S5MjHy6m3Tp0EU6mrB2b3wfn1sgrpCCr0InHtyL64Kxeqmb0EW%2F7GvLXyucKZjDggQOXcmpv9rJAWFIKaAvfWO1r8S7zNfejAYf%2FzDEQywHNNVmN5Q4ujR87XqKRR0sW9gXxCmOnP4pZcXYzGUlfcMrVJSyKtbP3pi%2BBGfX2dctsFy1Xix9LCh5qMPV%2Bo9atObTZbe%2Fxqw6gx1g5fuYb2KHxzZH112SFviUQjKlETIuOLqZtQY7%2FTo%2B%2Bw1uVOcaxoTyirEPD2fIhM4OLFN9rVJnghunSQqToDMgLRaPWGwkXHbjtHzA2NwJ0AD1wvjitjUqf8mDuRDfg1XDKBnMNhhc%2BhEGODhi9pRpedniGXAuZo9tedg49LHgc626bUdffPEYwg5D6xwY6pgGX%2F%2BsLmGy19MpLnfcCB9ZuEGeVgKzgrjC5yojZpqxJtJ%2F4MqhzR%2F2uzvsY9v3sEPWcg7MJoAZdL8pxLgh3z0pcRmRVyTeuCoTiISli%2FzvkIf3%2Fw3nnH69RxFdTBEpfAgE854rcv0WKDoFYkhCjmGXBXDWBXhVZmHY%2BeEr%2FnXI5xqh6ykT4bGpTsqW21xfZ5abdhO%2FJm1TC02zYUeqVbBS4EQmVdyMP&X-Amz-Signature=57e98de225073f887509b297cd0df57ac3419e53d103062780f6afaa59e55ef9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


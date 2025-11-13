---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEBHQYJD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDfvMR5C0BvS5U0r4KL18UHYkLo67sDOAESSLDWCBiDNgIhAPJ%2Bmddg9gSIP4G3RYy8XZ9c%2BCzPq3v8fbnjrq%2BVPO2PKv8DCE8QABoMNjM3NDIzMTgzODA1IgycMiiFDDjJ9QwD3%2FEq3APxCbe6bsDVCotr8a%2FbNUqm%2BDByKeP%2BHYCtrqVn2GBmYyqv2M0Siza08KYtxYSD2cTuiR7kn%2BgO4eKUvgP4Z318WdAvOuRxFfPYmnttXRWKtBUmQQtahwVYEdX4LogV6zJmkdjhyxU5rj%2FyqN4ywbVV3aWpZ6zmhv0YptFTevkgYL4tRU0oUlFOO4y5oTI%2BNXRTVr6LPAtYp%2FDSE1i%2BdRsc5PLWTrUAa7yAxJ2mzOueYb382Gv2b%2Fia59q87rQF3GDSQr0a%2Bp1ABIKpOQ2cxe3OG5GRdZxm6Ulp5LJXJd%2F2AnT8UTdidE6a%2BKwY50CeTq0VuBbNpE9nNqyJbxYSwMhucSVig0HgJjgNJpN8ICLeXQd7fv2d5F3OhpPpcaJGat68daBgBaqdPtzvVsBeSIeiNGSn77rsDc9LzTCRX13%2F7ExGnk7PsXwVFDPTJzM1NZrVWRCRecR1263ro3cD70QkmrJ1QSm1rTQCi3Jr8eZ741XMEhmR4lX%2FMDx7iS65kZLaJ%2BxqOIDIXLtdNX%2FIi9Lgy7Id0Ls1PAjndrb1Sdw9jmqOmNzUlhlEOMOXMnWm6%2BC8rP%2FaJDfyiqPU310O137XHJ2Zm%2FkdL25BLTg7DMhbQnkRYMqyVZMCuB%2BjfTD%2FwtfIBjqkASMjUQn%2B6Z4iYTbmisJ%2FFOgDCIOSDTo0OlBHDNGPQG09Hd6Yc2lgdBnMQ0sUHF0WA5MZRyRRytRqtmHgNjkFpVcm4rFvtzGeYJ2wLVHEK%2Bpd3pZjNJg7F8U1f1z%2BwOMmWFtNOP3wYQXxvHRZ2AZFR038pnAKir4IOFzS48IziXNyb6J1MU43ivcame01XpVPqsYXLNhMK9RjJfj4HCDhQRP189ZH&X-Amz-Signature=741defb739a588e80091019b34a57f4c83351584a88b11953c73182a5d97aca4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQG67ABM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBe9do6qoyKN7i7PervhuY47T3oMkNfFERBbUgM0euOtAiAT6PcOu1Kup%2BhYFIh3C%2Bv6xRCjoH%2FC05R71uya6taAbSqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMA3e0On%2BsRWQaTlrpKtwDic1qMa1ri8zUU2NbqiepKtKgyojFFDFxoH0DyH6Yb0JusokdH6%2BGy%2BSqkfxsMpLvCt5OVVB7luUFhHt6WAIHCwuD3a79OvYIpXJEqC69LgiSWLzi4Y0uCtlEwMEUYGWzonZ478qP6QEqyUtlVsL%2FMnLnCeFYepx1Ap%2FNnYNgzRe6%2B2breyyfqSTXOWv%2FPX%2FX6lFU9RJeuyYtMcpaQUqTGgjVqY3y27IjurHD6AviUwohE18O4xwxwtrEg%2BIvWEbOC0P2ANgqGJwS54nV4vNkee9WHcxi5%2BOOlaemTuiCvLBdoRytTDSNHzQl0omr0dWDt8bpX4F7gwrFDmgKZMFvNJYchB3Cw5rpaxDI7NBTvNQYI3ih92YsLLFAbImnaaVCRoqVI2hQix2xfzp%2BUzPZ8S40ugqeztfm6YffoPH%2F9LHW6T%2F%2F07Ef%2FHXJzrU%2B%2FbZOlxKYkcYNT3jKPhVJ3bNSrvtcY6ttJuDILtgh0mXH4FiMnf3C4qsonRc2WiJ8b6OpPxoXsAvQ1Dup8C%2FxKM5wgD6n%2Bws2zsy7XB0bSdHT3MaGkj0fjF1kLCx3RZE6wztO4JJL4kFaKW9B8euXixd%2FbC2%2FTobUwIxrwXjUYQ3N3miwVsHnrhdoueJ1KK8w4JTXxgY6pgGZq%2FAT8TGTbNI1bzFMuRw4wOQxpyBhO7Px%2BowX%2Fsyv6X8Tfd8Z3kZlxyd26vvELKhAiTUShyNOneno7cNv6MD7V1hJ3I1YlYwnozJoIpiPm4gZMnsZfpPMan8tH768onvPNWyLUr%2BMa9Vp5ip%2BX%2B0lCDcUrldIPAfIC2ebFGWzmi6fwekZ6VdHvDaY%2Bn1%2BYvZfUsJ4bcZyiYJLZ8rGe748cmxMOwCM&X-Amz-Signature=9528adb87918709673daf46a04f76211cdedee8fd46bf44443891a7f05aaaf14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


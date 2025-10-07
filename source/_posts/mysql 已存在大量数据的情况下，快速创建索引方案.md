---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7WVILCG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIQC3fGfHVeSZHSoxWAjuRSfgOtLgbRnJo2sVonsVheZMMgIfG%2F1%2FOkXkdDhPZhLgL%2B5jwuPEzIXpAwPWqeXSRIYwOiqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMiheRmF0Topjacdd7KtwDxd7I4FB%2BtYM0lUbjGQQzxBdVChDc3SWGGpKpfcg9DpYbARojil6goEVjnC0ebOQsXx1UHrsRyNJ5k9YSaG%2Bvh%2BdmQ2%2FwTnc6gHz86wkEm3JrCF%2FLWgghVHTGc%2B0GayzS4RgWSz5BuDE1U1jEal1nqPAFN5%2FJ21A8TEQDKZbbYRQRg%2FkYJMQ1LFG5NYLvU8NaKFvHVZWmJWUdrD7syqTi%2Bv0v8VD8p64E81g0nXSP9Xj9kiGHNYujabaOeRsIOskEnRX0QvO8LHJESGDnF1Wc0Q%2FTjiuGSdCrqadS4LWlFHzTGRgV%2Fb3E2xCpelbk%2FY6erG1ld0y9NIZKGlfGbY7LEtyFoezknnm0E4wCHvmHewPS7yKsfOyTBqaIvSlq%2F%2BFxi5DYOT67HeTQORxUboCaKCzv3ypBSN3hMdWhkvOaDhtwZdT%2F2en2i7Bbp1tel07HLfoVndxh5WDofFBiwnyIU8lfqNGjPCvaco3vFn0YtoXC2UR3h3iq7hrX%2F4E5MeZRb5Nvj4GCvkD9zcgBzArBapYPQLapU80mInNPI3%2BnnP8Yb7eF2IQF66GJJqkZE6vvHKliWKmxnr4or%2Bjg0e5mHdLGLTDJdK5dtTPFgoagMk9k13OKZszlTOAEH94wt%2B6VxwY6pgHekcD7xdtdHB8MXzsSNQwu7huFyF036W1ut%2Bxq%2Fmqdig6Uibj11iREYxM1ZBCnXC6Z1ikwZ20PQqyH%2FfyynGUr4A5LLEMDyXhw9k0jjzmOROEkS2gt1sZS0ObsqAkrQam4AXhqINieimh1W7mZptFv07ltAYgCoEgLI8%2BB%2FPCtY4ega%2BqHMfVP%2FO1pubM7KOFr03o4V0soq6JDhCouDlDzH%2B%2B4dkqF&X-Amz-Signature=8598afd93f37c098efb4af0b38a4967b466d22c10d6cec5bf8dbb0417f5be374&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


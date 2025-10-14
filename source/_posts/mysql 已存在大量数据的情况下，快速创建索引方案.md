---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VRDHPBS%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5s%2BKR9babMCwiBo5%2B3Unuja%2BmmejuxgqQpPotNiaLOAiBuzhYRe8lYrfkRtWofMX2cFgzAZQLtCYb8mwQKNzewrir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMQ8jVhospGpE7UbIkKtwDEuVUn7zL3djXC2owQ97xNwZeGF2mVPZr4XCMKr00Pl124hpovg9s5v7Jr81yeFfjH%2BNUmGEhjjWuLBWEOBU3iipB%2BIbG9uIPT6jQc9bgKbmWunCM0ovNukpdn8S63H6dMDaOgc6%2Fypop9pipQ%2FYUJTriYoBuWNlSQZJf5Jtd0yx35IHJWaAuwtf4%2BFKFCoMJOSCxFJ1VuGHV45h38axRRGn9SfQ2V7bagI%2BwZ87FtkTUDcUBP4HnBBy6fVAvJHyFKN4xTN5V0ISKpXX0olmTVif%2BOG3OPz4ixJsv%2FuEa3Esgi4agxbUzz4R47%2BRtBwYkc7xzLG6sItuXTLRwVM0FAna54dXm5yHqSdLJclagJcNRb%2FRFw%2FUceeJI7tYNklXpGmN42dAh%2F6OzHD7NoEWz7t%2FYKBgVEIAbkMGFP7BX8s2aKaQW5yczrix6SnZ0WZLJitq%2B2KC%2F8ban6r6hc3hsODn6xlXMSmkcfAMQ%2FsjHWNed5Fn7L0uORU3ztR20nGiPQijc2rB3kVReVlpDGcIioauLVXhJji5muQlYX5FNqUGIqJZvk63kmEXjPQSK87ZM7vt7LaS3FrcmRBVBSYTILc0Gh7S%2BlqHYLbfTVXcR2PhhRNVnXyfuzAKSxqYwtrq3xwY6pgHI89AhZNQxn8ZXQ4M60hjuI1LhmjJ7nmSwbOUf5whoKHtojQ6bLrRyJ%2BQj9IyIDnux2qsdHUwBAS%2BJgC%2BIuyzMw%2FgzeqJGp9r3Rilr88V3OS2smDQcv1rBp5t3C8vKXS9gv1c8Dw6mvabM3jYLZGeI6vF%2BcNs3QV%2FWu6QYCmpjGPB8A7vp3qZoEgtgQD00nSeoAhYQO01iWK7elUaKDQk8LDvst19s&X-Amz-Signature=d8362f56a7f459b8f6c2b88c82b17b607fed000e80fc7ad694425dd10865e6aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


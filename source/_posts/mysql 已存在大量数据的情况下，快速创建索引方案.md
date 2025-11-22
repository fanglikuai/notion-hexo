---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQZOVCAX%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIDxkSOaoGPWetFSBnk3GvPYR57P9tGf9uLC%2BAx%2BSEobjAiBLY9siAODTvtcYUZYrNduPfMnWfCk%2BcF0Dg51x4ClOXyr%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIMQESNSsrZR7i3eje%2FKtwDSoyVsKnPsTc51IzwtM7yzq3jCz1TP%2FO16A0RC71OzDPisfQ1rIEk7RCwOlBXsmyL2NBDAfpzt74JjKwbOhxWFqjzCRAWZq7jd0U7uS8kd7%2B4WjwaNp8qHS%2BCj0LSLUaZHAEFxYZg2GhaV6GiLQMxfMkVcaZVFzclLqK9qiMVETyjZkQkEeWcCyWHykRguNFRnFt41aYWjrDNYgufpAlk1cV1uKxr3WcIy2A90WDv2mVdM6ALVCfEDEiaG4X%2BA%2BNmTD2ZvubSy12uSSHHWiLR7NEs0G1MJkakRXWGBbSlLxmKABz7%2F9YKOfYhWIjrLAKtq%2BVeepsDTRrJVnKtTyYRm1wmlYsvMqe55D%2F1xh2OzSxR7YHysA8tk0o27KjK5OUc%2FUiKvWfEoLQKx%2FTA3wZzVDRczu7i3r4HrkZ3pJy1vJxkJ9eq7HJai3ukAY1eJ6DbbcI8IWp9SoGOYyaA2VPk4RrA71kR0qDfp7McfPuSEmD12uR6gsetmmigkv5p%2BGblPjkJlto%2Fl3mEXACgawOaf44%2FPUctQP2XC2iRUclPoC8iKznf2HnTU98j0QAYOjzc0kqTcknGzRfwZ%2BCQ1K9j3WAq%2BM%2FbkSZmrYt3DwRBiTmG2dmN2HwTmTI7Yg4w34OFyQY6pgHS%2BCmEDO6sIp0sixz2fG4ifsjyWwQ9bYuvDeMBVo8diKbaBm952UvGuqtmHwx5beW%2F6IswlgmNamjqbYTuF7dDN3aI6iZp%2FdCzhfohxRX8zQaZREOCg%2Bfl3YcwEXL4zLjkChE5I3tzPbmgVo7IjCYjYX%2FQfvXFRnwz69M%2F9e9Q7ss6tffyGPbNM5HUvo8f1Be9et1WofXHWt5jwNQA1Br4W%2B89g2%2Fd&X-Amz-Signature=39666d5a6df5d0ac5cf7eaca4dc12b0e7cb635b5d0707c84031dae5b3baab835&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


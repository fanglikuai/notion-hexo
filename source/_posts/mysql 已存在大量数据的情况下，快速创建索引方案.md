---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXTDXBXC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJwT2%2F0l4LN%2FCVRALZG%2FKQGULC1Ui7rZBL9BUgqWORVAIgDa%2FalpNVngXH416Cz3vT%2BVSzsytF%2F08QUEhLYFRCN8sq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDF1XdEBUAeI0KezWSyrcA6vOVGkM0OqTOLZj3hRHQnd8yh0z9fd1gXzyqmmCvMQRda0WAJRoXY3uQVYQQDWKPHlRuBOWghjqfBOTrNIU%2F3Q6jtXgCfQSjqonbkLwDAUz2Jnypz0aQ76S3QXnTYk3k1zKF9iuELPxIOQp%2BlSig8aUXyJNd058IElCBoJVjV8TOpGa2RxU9B0ppuTTKdDBMM2J11%2FuqUgNzFgEWicjfq%2Fe7ua5tRIrPng8KLw43r9ckcdWvcsFfFt6jQNUaQIQh6l5%2Bu1Tuxp1YT1PfCrzWV3yZIKIQ%2FgObubJ%2FhJ%2Bt1EqVRoShHKXzHZIagXnbJbSerbyDNQ9mn8gCnpQXtBnLrJHltyV%2B92A0guN9FBBxEyTT3Cw2JkKfg0sKVXi761X753NtxtgHBU%2F1E5DnHKKF3LFWSMNE3JUj61I79GPIemPxMVAU%2FVtizOvNkkToIXRmXuU9p8l3MKgEspwqAC%2B2BNVg3BIKy7gEl%2FsY8eZw9RAUmSS%2B4XULLZCCpKtOMzvlgleOxxstEJsPdNQMqHcPQp3RdTDPWE6D2GvqjkY41rhWMrn9C0nI%2F728SCnW1u8lK4MnrNwnIJZfHWUuDm7b7vkW5ZrztK9yBCcviyDomtm1seR5dAs7Qwa9PcSMMjnoMgGOqUBw%2BrFeeaS0GqF50VhMUZKIW4CDj8irwedOZzos%2FesEZvjytSTMS6BWW2%2BgkH7uil48Bz9HHN%2BfHG6tU9sllZyce%2FBknMXOMNSeS7KWZ34ubjYlBhndwWn%2FierHsOTDjERT%2FfVp1YSqt%2FV8L%2BeXPYdQZoJIjZrIW8rkMLubqshG5AVqom5JAFvSWmV4xCfGikTiWEJqfgCLQsYLLp8Si4PZ1yiH9FJ&X-Amz-Signature=b70d110aa2d69c805086aa226e5851619efc91245b2f059e3e8eccb621518b0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


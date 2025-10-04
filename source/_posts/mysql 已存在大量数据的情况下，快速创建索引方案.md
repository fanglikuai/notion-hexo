---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622XU7OF6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDoGS22BoBFajOXthKI7VzKs3YXKnhk7UMgkKHSTfxc5gIhAPcaChQ1f5uIdT2yMg1dMFCBPwP1hMhO1gSB3EWrivFbKv8DCFEQABoMNjM3NDIzMTgzODA1Igy0kpMDIndqvC6uU%2Fwq3ANSMcw0G0mWYauzql40%2FbHdndEU%2BQd9CQExyV4p6tLnhRgIs9KuhYrWBalkgN9FNSkpLsODcFYRMrjQUd3%2BA34HXYFyQJnBOpc4WKdYadCxVBD0V%2FA3j%2FtDWt4a8vTbD%2FZ90PFICMCNeIYhuJOonUixgEQCpeJzVIHlwMMhjIURdwZGl0GUzj1X4KplO7iJVajWA57%2FAAA%2BYCoo0lJbQTYzbXVDZXs7FDd2RWuK6NZvvnEKwMW9Ew287lH4xlBL9THM9ZTkyoztgdRWehzkn7F%2BB%2B5tNvCJIR%2FofUi2dY%2FzXTdQmsNhmMGAf3szED2j%2B2BvkRl%2BZyemUrIPIVxJ8B54OrokXOqHWmnMN4gfQPnkIevhO6f7NYun2Xs4k5xEZXzxh0topxKvJUyCDzbUF4rLwfHVfaqrYklNRW%2F%2Fg0wwZr98mSOwbb1aoJyQVSNw8qfxGFUq9YFVBqkIE0akGVfgAeZ5iGuuv8chXQAlwy0a1tqHYn2gHV17b2JuX%2BGVAW5FoD1%2BkOueDb%2BTqljrxP8krxfCy2u9WxJO%2FIT5YiSxJNA3caUqpzdLfAt1W1ITIplvGNqxW0lKkT2nLJI9XIUIQsDufHQFnCL%2F20yRtJZLi5JrcXGvJcTY0E68TDCS0IHHBjqkAVV3jlpp7wjbc08wDn76rkBPDYYh1REw1RGnG%2B5GHdDJrsmM6ZKdt3wDNY3AtLopwh0GBw4%2FdWKJAJGXr%2Fv02d14N0ml%2Fbp3SfEAmZGMSSHwYfG1alHn3u7yzzBt6ZjM92UNAH0XyEWlSpoGLsmVzDNZYkplCyp6aHuO3LBEolgJmzJibfijdTl6j5O733mS%2BECuzYIT%2FNCnnqsbtHwvxTJmhM1Q&X-Amz-Signature=d703060117184d8455e5d8a3aca4f2dddeb4d003ee8287e9d22dfed3a6e442a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


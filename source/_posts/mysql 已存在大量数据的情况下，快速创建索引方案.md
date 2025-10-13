---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TXFFI3L%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC%2Fui%2BKe2uKYFgl9HA%2BRPAj6BL1YRr8ucwgi3kgwB0iIgIgFkeVxBrEufx8axfRkS1IlnP4OVOZnKWsQ0vrjsY0Fccq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDLFHUxAXV%2F0goo6b1ircA%2FvnrE1HALh0UnMsuRAxfH5HDDPJNgQ03ORrqTZP%2FaLXR1AWBLFKMjBcRnMwbHRi%2FJkWlct4NliiSK3GvtPA%2Fc7ZfsDl21uovOSrI%2Bju97q36VjKCOKjm86L%2FHpcL37%2FNImJrMOGe7iuB9nHxv0k4IxVQMmD82XIg4ei8qqF1rxp3ZJ7HBgjgZGrv2oTJgSdtoir0Ebk59J60GWT%2FyAKZ1rclsWQjmHftDI6PyNo5u%2FjLd22hMbFv8wTHAecvL9bppXl7i312UtXfhnQJbdj3b0EYDkFTz5F1U5EQr21KRAGTtUu%2FEzKByktNi5FaHuGZnV%2BkIHehcVzZv%2BrDL%2BaNfabH9AKvESUYUZDjHmONAlFrenxTGs0ZoJawJ%2FG%2BzPpHgz2XotZodKQm9Cyb3uyiQCjLvJv6yxUp3ov1SMNULPHDai0xu5kj3vE756dhfzJycv9wnRVPJAh6IcFMydRwNbmaiQZgOUm2rell8PtbAEBZ32bjPRspyl%2Fn3K8aQvsqiCtLGb6Cv5RbVxnNfOd1BsXot2jQfCIiYF7AmqOdIjHQpBwUtHl3kdXtTPGitJbQbnBanLEXt%2FD3h6%2FxU4IvvLohk0EVSFRDO2TLIMONR8lZSJ8PZ3Xcu4vw44eMKzUsccGOqUB47GnWbJzA0J4YqeKA1ZORLoQQI7pviZVA0KTdvq2OW2aG%2BGOgfD69x66QVk%2F9CxZB1TcWcDeJbDxAXe9PcQ%2FSCjoSSxIOY%2B0uCE7YwdqK%2FZJdUT%2FCTWbLVmXA9lH7kE%2FWJlH8ZmgqdLeo1poM13wz4TK6KFi0WRqhJ5BE2ptPdtpL36uwtwxjNnLCfo%2BsjxiG6GQj2RSBT5F9PV4kmYnCXYl2%2BM9&X-Amz-Signature=c1ecfab0816481b953cbde2d68e228b128f5c02db4532c831a81da8b0994aa60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


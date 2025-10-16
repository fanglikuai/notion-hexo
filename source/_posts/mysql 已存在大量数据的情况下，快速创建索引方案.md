---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LVP3YP5%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDBCJm2d2L0%2FZm5%2BqOMevPMb%2BkHVps4l7z5WeLF17qr4AiEAtbJ9TCyVxh2SNYkCods2gt7Ccy7TNkHXiv3tBqqGr3kqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJohYy7YV9lgedFXryrcA05sNCO4B7lI3t%2FA%2BeCPhJMNCnKYUy%2FGHcdr1LDYXJIL2fZyuz5sxVIHugp1eqxQtX5skGrsVPleg58dEKXRlwTOpZfLZKBupij%2FgcTpiaBOsji807nnPcbPz3zwkGuQkBxH7cAAdfdDEHfnaR7BDNAMTgq3nnoWD73ED6W3J88HkKCt8LgUh33LcEUr6KHOReWaP7GMK09ZQ3SykqJkbMzcZ7lzIxlY7zeu0mZooQMSMV0oCj8NBRwEdqfWVsO4Zdm5I86hK3A8XdqPAmhWkCDLVt3%2FCBxJKQvktRY5A%2F%2BVRuzLtQmzBNJ1i2QZN55Mli%2FNBOD%2Fs%2FcC9WrYdXzPH1a5rakV7S9mrd3Aq%2FzFxc055S1zXqRdX%2B8N%2FLGUsuIIpG36qXVLgbuuEhaUU2QUXoGkKo3YVZgjszWSja99vlOUOxdav3eeKSYYL2bWqVLYB2TNxd%2FoftRFTfiyvnlIO2G0vow8SYWy2vbMW%2BODNYGXUZaz0Qn7Oz%2FOIyLoWomMhEBuYJSQkCU%2FXs5VvWxyqrCmBp8xRpADfFQBIWpyHjhgJIoz8jQy%2B%2FSSocRUpoHnzmPD9zAbt7j%2BRb7tX0npNbuYzbq0Qyv%2BgUoxUCs2gozDuhkY22Edi2ADJczqMKXawscGOqUBdGsi%2FWvFKWd3N0yYDuBvRpDLr4Il5MzjINBqzGvI%2FlPOdKwSu0nLOpxC3bN11n5oEY7MmLO3N%2FsYX%2BM7%2BdvWNHqjvpdIpCTX3n9tcb%2B1r2FeVETolG%2FzF73uuwihjwl6lagTawsWOEqA9gp%2FugEXInPo5goZlce%2BYH1U7CfqeksX%2FApNOsgl0sttErtMD1T62aQM2nUaI9atva5V%2B9IuOqndKyMi&X-Amz-Signature=b549408ce97f9196f845fdd781ab3e9499719042544aca0372e6e21eecb8b189&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


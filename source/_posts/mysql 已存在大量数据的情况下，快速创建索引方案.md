---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SA5CNBHT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T130043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCRRZIm%2BTQWkWWU%2BXJGAoK2Sb5EEFtacD0qi8bGsoJuHAIgIIVcpDlW%2BH9ePmO69U1v59ejHqmEIUKs3qjyk0DovFcqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPphmo2Mrvn3ZMfNvSrcA74QLQFKuxgjOjmv7OslC1lyZlgSSlm4%2BESXE1dI6iDrP2VJI7rE%2BZKARbLvUDD8YQJEbRlGSB1cduSsLmirffyZSBgnSBSPRKzx%2BEevGVjzS%2F3iZ7Bn%2BSHYsXUEhOqjjbztL4WygUfr2yb1%2FewKQvTx55CxqKBtA8mk1%2BpbBxpSqExk7lVO79r%2BelUxWNWsJKBofA8jAZ02CNhpNrKv4ozTXSmTvuu2kxoAMxYf4qlBAMKFS52ySpQpwe%2BF787kGEmTVXMzlMnNTomnDUXN4Fnl%2F2NET2%2BQnOm%2FQ3%2BvkrxCldCjXERYYC007O4UL2Ly5xddRM6Joma%2BofdNXksfLM%2FZbi3xdS0yTbnlxw7zE7BOL%2Fa1yAUnd%2Faav145WBU%2BvfLzecEsyn75wzCMNDM7hMelgw6cgR1BIlcrftPDesCKtH6zorkgQsklaIZVfveCP6XefgRU9IFcpNeDr%2BaVofyOG%2FgDo%2FO%2BhBvJUUF%2Fb2fWsQZaqdPSmZ1InNcObzubMpoI5L6N6PuXGojhVPoRm%2Ff6raJYkvPFGt969gjqfljH69HNoJ2WIPDzHRHKXhQmMbu9ZlRnxu1tt94BqRsiBG9AFNAVCrX4nQXLLJo%2BC3D41YGt%2FwW7XYmgCWSsMPuvjscGOqUB36IGWTfM7BtzhcO3acCwRD1KAY2NA9Is8QRkdnLCLPUpk7W5DlsSrH1wGyjvQUB18cICUXuhJyoMifhY1DG1M%2BaD4pXJElkzZRXiVfZpn%2Bm58s90FBSLJvxmR3xKqM8bEWZ32a%2FxymuIZOdCJ59weG1XumIpOcR2iUoQoUUyswWfrHoJmAVU7TywWj16S%2Fp%2FtVgj01e45EGQf15iZUkWMj3xWsUS&X-Amz-Signature=cb5219836ed972932636d3d29f77ba173b2a190c2360eaf3d600787d9e646b10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


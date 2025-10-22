---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFEPRFWM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCOLUeVoL3rEdAsrWq81BIffmnQNFRQfWRamrME7kMlTAIhAN2mrzdtgw0g9XmK32W1aLFkt%2F3q3Fnm7A6msracTKsOKv8DCC4QABoMNjM3NDIzMTgzODA1IgzZQ4KvGmp1irPWTJEq3AONfjAtxUu%2BN5J%2FVKJsJ%2Bn2jeX6WGBKBedqR2AXa%2FbpQXH%2FIlLTGSKOewWra%2BZ%2F0RnuGohxg9Uek%2FBbivHIFwTkfRuV9aKLYt%2FHN%2Br%2FHkSXocEcamdA%2FVSNg6RA61rs5UYOA%2FvfdZ5Bb3yUVDeuDwQYapGh6kAXgr3rSXczRJc874XlFDcnT7Zw4Sp09Vt1EHCrGdR3jUljoOVy%2FH%2BYPSJ%2FZLWgbAF%2FDXpMkH7m5tdkh2vIRX41O47wAfYmcPGkPb2SJcifPbu%2FxBOGZjH4sPwVxR6U8H6%2F6oSuJ9UESSNZLJQ2f2v23A1BZ3jU%2BztfDwfBW1HLFaZTjrpnOyq1rBvFMvyi9sQCXCWcomoTtvKDPoEE3BmY8JAWgbO%2FYHUAJ6Y6dHEC69pthzWuaEPG7WD263ppuwcN8raEpYI9tTvLdME9WRqIKq5wA%2BkbPwCLhrESqc4bYLl%2FdM2QuuXC7dLwO4CLuRPRJR%2F2Gy1y321GY%2B88%2FUhjpAbvIGnY5DMRNcrVw12xOv4pX9EW9GvjE%2FEOL%2FZIGO5YDS3v5gUGQx7f1Pjv%2BAzf4XjHm%2FbqQ%2B%2FdZ%2Bs%2BQ6paJFo36YgO8meb3b2re7lR7IRXVTEQxGsAETDeIENSwLCznGYceB2NczCSp%2BPHBjqkAX7wMUaJMJw%2BKjdTxS8%2BT%2FD4W1UOLFjLDrNWdz82S10G%2BuCTVEkrbBqmr14A%2BmgcIs0iJeNn3%2Brdc%2FQwCpkRvIXBHywpnUs5TEiwJ70nWPChLkm5fDzlVrdvivuyZ2Bbhcafgw6TZHKIXMw0XjBfBTqpeNAnYA6gv%2FkeUrRjeH%2BZpDakHo1diHhVOteAuBl0FIcGBetuYbDDa%2FCEyg76as33ZhBT&X-Amz-Signature=d4ae8726bd366159fb4ba59317a843b67f2e3dbc1357a1ae1f54c8f995e6d6c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


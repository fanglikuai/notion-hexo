---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVZZBUT%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDUECtOvawN%2FNG%2Bnrkweknp5iKY%2BX4ZF%2BKcGvlI0PsCIAIgQU1hkb4tVtLYY7ph%2FcAJNaGQINFTbOIWF8iseIEXnJ0qiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNEqY%2FbKGQu7adOPyircA9nCBwi62jGWdUn07iabuzLCT0wKISAPq%2FBqM99FVTCSDFh4iXZKJvRxrJZdO0nnuUrREZijOe2X%2B%2BXmsKMX2c5O6oOevEwLZmp6z%2B%2BtCXDHC4Iqx%2B4WD%2B3%2Bspp3vZ8E%2BGPhsamsQh213qtlGOHpg%2BDVYne2P9cR61xsOUo%2Fa4saP%2FBY5ahluV7s560dsZ1ioe%2B2rmboEUp5M%2BOtPjFZcamGVtsCbTnr2vCcLxoVbyYhJM5UPmc%2BHqWDocRWMvoErBlygamppc0SANquLXq9TXWEyIO%2FEqtxY4sXWr66nHBWxy8U3rwBfaUhxiLaq%2BVYE85ocALf5xToACnx%2FZrKmWVz4jzkbdP1sxt82AfkNvtnnwqcz%2FPbepr%2BuURp%2BP%2BRtNSfOKtFog03jImsmEqTDCYi6ZwCXyyfxL0TidMX6dDT9n8Uc%2F%2F9Yw63M7Ar5lf7eLeG3z3tMXDWR8sI79Gh12%2Fx0XqRW1%2FHc5y%2FS%2Fl532TQP%2FUJ3a4KhphTib1Yck%2Fjew7qs4MQ3n%2BUt0BAnLqMtUBnm3MK3JDea%2FaQvKI7Mgz9KX9ILUlIgnAI3G2NKIYjNh%2BFp9tESBpiYbCX5cArK9fW3umFV4TYvqHPu6h6syETPRm840fUdxRMY3SdMOvFoMcGOqUBsSUZjtH9%2BnPffN9Z8HUUgZlGveIk%2B%2BORMr4SxIkX5i%2Fg%2B9drYozdwbvG3iZZFjqvw%2Fm77F15m6M0Wc1PG7NEMigWQVmPn62l1X9%2ByO4aar0MiaiRcvQ6wWBBYXn0Yn40t1yXD2%2FkdMI9DgURXEgr5BUP95NfOiewSFBNTqT%2FL6KTeQhmzLSzUEX%2FqHbMf0Ef630E%2BFyWxaDFqU92xGPe%2Fz9Tcky%2B&X-Amz-Signature=8f124ba31bcbbd5b490fb9113bd6029bbbe0486b280124b5bd8c9a3be088a5ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


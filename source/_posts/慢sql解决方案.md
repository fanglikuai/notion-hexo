---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BHF7DD5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDISBXiPGPzMoTyh1fRW7TzVqKN75BJZsV6U5wdh90fNAiAhmtxumBgLzpdW%2FEObuS%2Btm%2BzvOY6SExD4tYiwGUWPOSr%2FAwhnEAAaDDYzNzQyMzE4MzgwNSIMkVSRsv%2BiIvdah6TNKtwD2ixj9tPk%2BYYf2dwa%2F9vDHAXhE%2BSx1RRXLralb5mvr%2FdsRDkXTywd21h4zh4z5CLyCcujs0fg9dBS3QLoejMmQCaIlhoZ9wzwi%2F6poeZpTQs%2FQ5rzMG%2FZHKCaYrIt%2ByWbu1gPoAQff6KP5Jmrv%2BIIeQDJMxQJBpTotyrfNsUZmCQjvPMlZxM%2BE1wkimuu%2B9smgOE4SfBoO7rhpPfe50Gtm8mp%2FR5%2BbVmJByGvetNgQHFRQ9f2KhMRu%2F0ZWkndaChTGD7jHLI%2Bfgy%2F9B7ZcLC2a8aVt%2F%2BtL8Sw0ymjsPj3uhbZZnB7%2Bb4%2BBWMUcql%2FU4QKAFLL0aWEBWPCC2wsZ10wQeYt7VUevSGYPqHh31CQgkBO7y8talfdVA9K%2BaF1i1mpsUiNyuC%2BPpt519Eny6uE7uR0jDm0X3nVFbaQF%2FEuDfdODib6%2FAv23TRhvA2M03PTdeuo4eUNlA8vO1Ozuk9iO2qJ9xE4M%2F6HLNpDG7gbafDLEjMIB4Px77c1kSfdtBTODvVHzYFJSmsposOdWK4m2%2FRAqsb%2BRpAprZor4rwLULQriHXFGoogOOq4VZmFTZuhJeOLrI9Rec0cq4xaraZdYZTaTHLvz0Us4yInyFL5iOO3i8KlZPhkM1UAmhEwvrGGxwY6pgFRCFuMYILBlVXehvgaTViJo%2FGdnnC9Qw%2FGMpQ0bUJi27QWjTZHm3ZhqXXV502CMgeS6PaXIk4IrPCO2OXzIck4IWyzChXZr67BvA8aaC6bf48GenLe54UdXJiohJnoepb43Ari368hD2eBjz4pnJlfo2%2FHmnr7W2Kz%2BxwacX8ykFMgVFATFXZr18YyARjhzUylc8Wduq8ZWqzR%2B%2Fm4W8%2BgzihffZEZ&X-Amz-Signature=40a059345138c19310dbace46b5fd528ce883f817474803dcf6575ed5fd1f892&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


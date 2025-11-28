---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662NBAIAH3%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICM6DwVY7S2ct6FeknhdnA%2BcdLJgIeQtei2TFdnf7ilsAiABi5FFiqBg4BZGG4aW36pUPVHpmghhMr7RhgdWn4gIXyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5WCj46lnq8eMwhz8KtwDEz8jQ%2BpWiNw2ccxS1LWsUtiGUgUeBwG7C4V21Nf4ySoBF3s%2BkI0D7MVx3F%2BApLxHuB68ZPWdrBYK23vir%2B0BMzIhIRGacFHL8t1TNKUnOlnNFJcQLKkAsIXWY1iRpixM6PIHfKTbboaAgyDnivtaeB8oSsa%2Bp14qYj7cVnyPr8Nowc1gOtrKNDn4r%2BohRhAryX5Hgx7UoFWtbI5dVMgnVyzDWFInWc5plfHcaQ7dHVlwzhjcEXZa0zMaOuyWD4BpLzjQPpn309quAyR42BPZugxVcyea%2BRvXuMZrDjtIKrIaEN4YoKOIa%2FGwlQowpMUrpDUjJlceTaT8uWff7OKbUSpamgm3fuLVKN8Uq0kDL4L52iN2h4FgWKr%2BVSGBiYu9f0H7hGBsesbEz%2Fdm6x%2F1GLczFgdpxnYVRxePnWQoYQ28pcQZgVXej0rx8Cm8gf8kA4J8AUm7cs%2BAi2%2B19BUc36OsSQEOqEF5sw6FF753HSaulHRULzjeK8W3nC3cha7YxJBIzNQv4JcPwTZlIxBizgfLLLTWMiDW%2BtkbAE0A8Rn7a3wbz3MMTOWPbSxCsNH%2FTQ%2Fqt9%2FrWu4k%2FHJ78IqFz8jSBBdznCOl3knQNgrYnZhurGd2jj9C6Kp4EHAwnrukyQY6pgFARS6jMyGd%2BRK%2BycrYtO6fQYuQjbI1vEoDQT%2BAUtg74W0hO8RFwrbo%2FnTiAAP0VX3nM28fEcYwxGBw51SDiIf02OTKM10yX75EJgeprNpI2NtOAP3M2hb0iGuEg1rb%2FlNo%2BP77mjYZ7lEhh5x0sEhSvZMxZg5oTUq60bLqZhXkSNvtQtUYI1m0B61XW2OeKz%2FMvkdQ6da7wyZRkSC1%2FycVS9o2vrgg&X-Amz-Signature=6660bf7ebcca6974fd2196dd3b1b73c7284a8086ea54a5a5acbecd9b50c96064&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


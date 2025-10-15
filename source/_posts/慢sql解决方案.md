---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCWYNWCQ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1c%2BF70vQRD%2FUv2zDNTM8%2F3NdmP4ir3IPeA4%2BHsue8tAiEA5FAf0W0%2BtV64Q0ed%2FXiaRONC7Vk%2Bg%2B1WYNXhD9u%2F4sQq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDHk312%2BfFJRUkXYlgircA6jAGmEEUhgSKCnkfxGSj2XGx5a0LMRD82qlrNDhh2sh0HJGqDh4rXlvLFkseRCITp3VlBZeor51nyyIChMamYuXq69D8Lb0vYT5qg6SC05R0%2FkpVhBdAi3fiXiks3npz%2B0uFbAlMP246nNEVefn0j%2FbA5th6F1RT09fdo6Xx08s2Igw4WlodjLMA2zCkcvS%2FKMmyiO5Nn4o8J7E2AGGJStAIGWAPBUFZUhafCxSrbJOomiwzo69Jxip7c6ZpbtZ3UQ62%2F5zBvUYd2FrYNloiiLHvAoVxM%2FIS36cRvt9kDoQn4l3ULCnygUPKkMiIK5KXSmHYgjdjoJwl9pG2ZvDGR0SspaHPD1cFxBTEu9Pmosy2HjtdeRS3WM6Ulgo5L16dGAKvczsOKI%2BrdzKVa1bxhezY66tNRiuvqhPxQ7ZIKWof%2BD9CMvcIvYgVNFtnRn4Fl4Gq2GpDu%2BwXOcbP8R16A3eZRTxzPkGuNhG8nJ%2FOkx0Zx6hF0oAzFoAwNQAWc78xPAC3%2FieU7FL2%2BuXg7a7%2BXV1KH32r4NAuy0s87imEbFF98eaOZ4665JDJpy0DLwijZZXVwJiye0h2BdZiVFplN9cdzlvgqfnOGVGyFXAcPFBFEL2NhLQ325hqyQbMICQwMcGOqUBJBrEG32whBV5xPCNjlEaHJHsFLXCRN1SctfmDgJmcSv%2BnT2ZH39xAvgOHH96RChgoOI8pPbLtbDXQKhViDKvegZV9QqtTgHXX%2Frvgje40%2BUdDylhXyLOgXxOtppcsyHTAWgRw0e7DuTjBPts4zG4855ebyuNt09Us7irIxM%2B1OfTcVkryH2XxESPEma2LHHPEyx3ui5v%2F0bESgnQL%2Fxh%2FjFjLUGz&X-Amz-Signature=df679904eac30eee004da4cf9368b228ba8c7c24400171839ed55da303d636b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


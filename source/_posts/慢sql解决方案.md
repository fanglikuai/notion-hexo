---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJ46L3K3%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF9WpGMsrtNZUlJ%2FCZM0NUZwhVUcp3QYGMm3S%2By58bQtAiBFCXMP7ZSc5omBzYMh5PMG5xDTFmWy0djoH4epzK2TzyqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxIrAbK38sbJAJl1kKtwD%2BQXfcre4uts4UMhZW6icDp9WimO%2B6ju9xcSpdO%2Bin1Luflmx81omyWy8GYBf%2Brb4mMGakFQULpDQVgFEIYvDdQze1rK2Crs6T3pNudp5rKo72IFBuWR6aer0dghSqtzS5AtX9SQjVyjv922xeRtwVw4B7edMjm6lGgTV86blJHPQuVg9TyEAF7cZL69SvvT7kmteDk9L5k3oDS3mRPmP%2Bw4vWZjFm1ZJyZ7BE5n4HAXJ7dH%2BSFNPNn3yHwhuH%2BVLb1%2FwRgdzkPprv2jl%2B5TLqsT9T2FKFWdN%2FYMiZNuUudV8ohi3npRHDVI7ypu2rC7GYW9IbBnCanCLbdRAMYijlDm%2F8JerK446VNnytp9HsB2bdTsguswWBr%2BIH0Uagj7R3nujS74doDEKKd2Yz0WG4GKiLEP%2FAalBC1MuprH4Fv%2BmRiZABi%2BE9N2YXyQZE825EvnFsK2WqysuOVAVlUbzMtOiqDf%2BHXBa5FJ9SBsUNn9AegbNfEHwhgqm5NsVwIU13O9cLTMmgIDHt14Zl3VN8x9dFKKGp5s0W6ghj07H5lDud3mX4fJbxQlxca4jYvpCv9%2FniNF1qs2Mml5OEsw2rnflxyqn5Z4XcFMdu1lWRcF9De1yf4b87GRlgg0w%2Fdu9xgY6pgEcO50AYN6Xq8XCMPAUZ1ZM%2B3f8DVBrhfyEJzZ8v7KuQTXt2WTH5jYawbEigJDoLe6Os58KSkTmgK6gybKcABVKa%2B8wraRJDEFqDxqpgKWVMgJRMm%2Bg9fsoHwIzjCRo5sp9m6E4rpmuLk33m8tzCtg8EyLc1gpZhoKAPL2zGVdcty1C8B284OMDmsLaA%2B27FuWcKXspXcllQhbzwLxoJwFbxQAdbzbL&X-Amz-Signature=dac807fd9e931c1758c830d9202d741fadcb2ee73bddb88d58e8a358b90f50a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635APQLNF%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIDYwVQZKcvNRcoMHrdWb2nRPZvKPd9DIwsEuivbeQ2RnAiBwGYgPpdKeD4vi4M7qtewgL%2Bg5sriKdhmY41vgoSiOFyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbZEE8mmAl7UfVxi2KtwDq1FPJnXAYybmfXLbwH2ovDcnZi8qnOfHhSTEznmquNDZ5qWWsUjl82vYARzBBDTJJBqXC6u8cApwBn%2BWQuGbNNqRxJLhbLj51a05SuW6h5APZ4gN%2BMWWnzTZGE0D%2BZja46Dq3E4FgJIReMhSeYSDhExUEMjXQXmNH7%2BqlVkJ9v19VjI7bK3ApLUmwWBEwYEYVgRtAdLLHbT%2B9XGL4mvEiv0KEEoi1VAyF2llZy4MmeLTyqLoSm89rqJ08whN0aEQrEJcC%2BlukHGD4rMKuj%2BelM0OrrWkRYuBtmhLLEsRZZgerDtApA5arxSBsIW4Ky9zxxATbWVIsMSbUJVTBRjiI2qRMjs9fpE1CeJVSsoO8T16hQGtm6mqhHEe9No%2BirG4aLWIdBtbC8RWjpa%2B6bE%2FLbp%2BphP%2BSu5kyrUZ8dOVKrhdyQyIftCVB90rkta0isggjI7gI61HUjMucJzKghd9oJUPN30MGF8Bdf693JPE6XWlLYyKW7w3mwDO7Y5a9N10xJTCG%2B5zvfH2drnX2XJJpAFlVFRJBIUnhk%2BZyV8iRAGaz2HsW1v5M1DvDOvfsTV9MqH0GuKedGWyHdc%2Fw2O%2F1KDj%2FDxvDq3nsDUKtiB9e59NCNjduSHUZzIEt08w7PCSxwY6pgGhm8OAdY5FljLIh%2BCEyC5R5bJnwylCkPkrWqjILYauq5j00PBgDXYJ8%2FJiEDrUVPggPnfzy2v1Syv0c%2BnkqDmfSSNE81fIGaYkoTfwk3Bq7p%2BabZNyZNO5WG2vLhaSYJAi5WZakgfqVkfN0J%2B%2BOnn7%2F7WSibSvvqvY497H1i3v%2B%2FwXX5sxak%2BZhU6aw4cQfMK0rPQtNk3L22o3nvNKiCkmPCBD8On2&X-Amz-Signature=e5278dde61528f383d08adccf6ce8ae4fc36f9e257f8733c2da04f5793fb316d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


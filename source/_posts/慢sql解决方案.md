---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SU2GOCTV%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqhl83%2FjoO%2BJ%2BVWhgURuqCcUUYMcaHsooh%2BIX9PkQnFAiEAsDdipTvNeM2KSL%2FMmPuI0PrrJzIbehqKlsPq0NEqsQQq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDEVzYSNpkn0OEkwlyyrcAz%2FhwOkBwZdNeIgYCg311qtM%2BSDZOV0cZw7nzAQt13MmHC8wYTIff2WaXBsT%2BEgD5ygWlmpzr2ZotiTCSuqlJF5UXILaF0BSrVS5qX%2BuWINVEZ8gqxs7CuH82xaKvfbSkY5EgWUaJLjLOjy1y1wWEkZOH%2BrYBAZHGVq%2Fr0rpU09LfX1GaGZsQJwTTrANlmLSCyxyVJLbaWXcED%2BTfnYDu4ebOibuvlclVefry1ttHDBFTn2pepPxd%2FBv%2FgwylipdN6JoOfDtBM3yQ%2BSu6kTruU2tegUjWQ10Z70DdkacB4eBLvHg3TlJ5D4kluvRtvFAYMGxG7Yx5kfYXWqrRzhqNbxWQDvoIgOO%2BhS1QmA%2FBh7JodM1ODGcF%2FsDdNULXAkEtSsmct%2FhLrHG%2Fjpa6SKlulVxNZ1uDgrwLVkuPY4KcpI%2F%2BQUVUZlzC4lL51EOmqI%2B%2F4sw5%2BdXBsZltCr4VPKXWIu8fGKmOsaQ3jeZrRhOMdm4vDkniIx0oS22dFv7qJdHK5w1SaNy%2FyDMeNGIu45tG12Roh0d4%2FUR7sfqbESA37%2F%2BIA8Vm3SjWlUl3T61ckU8tirKgvoeV5FV3jp3PhSSmlMHYyfwLP20znQCSnA8q7f1CJuMZTS%2BmX2X2PGNMPu4gMcGOqUBqeLahfVivbpolElVQ3T45cf0e%2Bs0Kg7%2BMrS2hAdxaI1lHH296KcY8oCj7o9lmufHrFQoHmncBZ63X1La3ViJ5QoKxacvGAX1YhVjOLwFuebX3TqWl4e3Z9iiRqEVvU2vB16RYedJtoqI8u67RmC1gIWh%2B%2BXD8dvRvewxnpiuqDDyTI1qrC6998JXnL1P3vgOm0a1Y8Swi3xa3cUuBA3dU9FJfuaw&X-Amz-Signature=abbdba3b0b2bc2f741aa2aac1a299f95979fc82bca1a43cb333157fec6b7e497&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


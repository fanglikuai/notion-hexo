---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNFWOKLY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB0c4dCKuXANOCyVjhQtb7S9VqNLW8o4jgehwslMQMZ6AiB0bWLM5i0sI0vlR0XG6%2Ft6w1miFGq6bUqRUANJqf%2FCfiqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMvLgCQE%2BhiZCztsUVKtwDeoeBUxsKmQfoeDD%2BU%2FJY8h7lexoSUHvJ2ixH4RzvQEj4jbXnpfpW4PxuVhiW7dvYNgV8nKV1UDd7LFNp9%2Fe6WmLJUvObW7%2F7CEwyKKk2wuWjpuNezl5ZlO6QZB0ZN9FE2Oawfy34HZNnS7MfV3BCRBF3Io%2BWfXz8OOczXo1sylUhAMJmVzK%2FR3PV4964vo0mHBdtb7h1zK1Wm6ZSgyWTQbxEfBocrJH%2BlCHgEc6Fu6gfpvz31%2F1jFCq4FQ2mDUWCXKcS%2F%2FiDpT4mgSAZaDn%2BI77t0HVXCTPFYIctgpsOyCdBy8OlE34IyJSWPVtivbOnNgnWjbtY603tdaIsI%2FFLE%2BFZHzm9isEYP%2F1cQMWrc4ymysl%2BWcPQ0IN6mOKDaGe%2F3J%2F%2FAzY%2Fm6QDeV1Cx4aL0Y0wGHpw0db%2Bk4%2FtNDWK52cJghzXodQPMuPgzlNA85n5AqHWIluFUAJmfo2gfBV1JLsTW9dM2qQUrtk1W%2F2dYtldGnX4LFsPlw9tPtMZ98DDLlM35fw1YEPhVDL1uDV42nj1gYkhuOA945kHqF04UyLB4TUKvkmwKHs2mHvSIkX4mTADDPivxn0xjhDB%2B2rXz5wNQTY%2BvxvQc4hpCWEEGwpbEQtM3FQgarMUWvwwnLyayQY6pgGnARoJv1gCf2lBoMyDFMtTj76nPg46ey6uLUifs66%2BK437xiVjQb7uScnLwbm68CG5pB2UoyWkUvEby%2BZCq%2BBNXlpeVm8Iq5QCoF0gCHwkgfvpHxM%2BebLDsAH5ovTn4UtLyXavqqhdUXRVL5K2o92fmK3wGe375pt914SYDuJjp6YNafPaS5tEfi5jgGxovxd7RIgV3r9%2FjHl5ZJ4JenfD9bbX%2F1tb&X-Amz-Signature=cb158f3e33b7674b32fbe917153f4d40fca7eb8ba46ed4c57e2be2e76da3f6b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


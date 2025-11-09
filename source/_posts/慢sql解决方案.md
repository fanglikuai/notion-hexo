---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTXQIXOW%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIEljmx8e%2BCRu%2FasRqfasYyyq7Tu5HYMWo6WHVIz9HGU9AiEA%2Bd6cnsDQBpnMKzRk4YqGrn8Ap3BazSd%2BnKNF69pkys8qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH7tj3adw1SofiChZyrcA2iYmxuguq%2BrIdohUWD5upAuUdXy7rQj7oXkU8RHVyYoTVf0%2FRGXMr7tPXGUfQJAabObFpMBEToF3jYXNAQ89O8Y0Tpt1YFQ3xhg4mit6viPjzfgYhjsTTxIa3rIY7Yj3HT8pQvbWWWF0we4poGcF5mdLduZxmH4acznWOnXXB2v3X2yBniV3bm3vOMJrqeKGkCYItIIFhbnjsevYP%2FNylUiuIqwk0mrOm9q2gpKE7Ig%2BdKnzKqI227%2FKEGe%2BIvOVWdZe0YPesLsZpFay6CPF%2FlrXpL87uIwggpj5cAjvbwmIRzURZF4HMZCg2w7cwxpKOJ7guODeegK0Icslk9cPAFpa9s8CBwTFqUMDp3%2FYUCa4VaFqZHtyg7Slb7g%2BosR%2BA9fGsDXJmYtNThdPJPs4Hsu%2BidMuYAtvOFwxshCCWZWJ6yYQShJfRN7RkkdNBTISb5mNk%2F2gYoaDuEUoQx46bOdvyrbdqGcqBl65bd%2B2iisoUoztRvluqLE6FtWLovscN3I%2BQ%2F1JdTUXHZ6iS84P0v6RUN1edAqKhTdQhEOrgY1EKo78IuEb%2BJAa%2BqVXmduQXkbBCbaQN479cxyCIftIrUbRh5OoV3RyqTOboC7v4W5sQ1qdRRTEvcJ13vpMMfuv8gGOqUBvlXytjhIn%2FgK0OGxNtF2WBv2BhC02hZjwdzDmivIIGBaccHhZZQjphHJmUWwgFmwbcKEeQbnyKx8N%2Ba%2FKkAjzuEnCB6K1kh92haGqps5NqCZgG3O8F6X3k%2FRa0w%2FwhvQxsCEWsX6ZWjRNmnrH%2BMdvcklXJTWlcHQIEYO7m7vhgIvhO%2BZiO6P%2B3s6QSBTs%2BA5XQl83ekYmdMvuS1rxzWv3L2LDmMi&X-Amz-Signature=f91929f493674acb56d2e9ef5981b9d76b85299a9a14d338b9b397d520726f1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


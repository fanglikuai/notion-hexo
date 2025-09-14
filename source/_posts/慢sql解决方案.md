---
categories: 数据库
tags: []
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZCCA3ZA%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T125005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjYPByxsu0edPVV5556dqW6S2dI101rstjQPetGfiQ1AiEAnunqZvN10OiWI64tHl%2FxeDurWlZFGBVOusQIDkzYrf0q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDPXZmmC2dYLIpIjjbyrcA3jqw7epkyLYE8AT%2B25YkeAEfY5Qp2mWPQVRJg%2FfFs%2BDlHy2mKG%2BjUauPHgH8SpAEoVr4S3NUHSxYlrvQfbBMQ4VxF7PdZcQDAuvbUlgJ9yXlX16%2Bcc%2FQ77AcsTHjlOIXzYqTAApk8WOO8jo1SYWf68%2F7uQnXFFFfYJZiIXhFEmCht9cmTZ145Nh%2F1S0nwkPfZtNHt7Ob85jYl4twf41UHsDcSPZKdYPfmoDNLeNyyfHW5M8nK2gS4td1iedeeJ3qBOwttJkEBBp0L97SsQbd3z7e2Q9yvRaIVHixoJZbSj8s3HKHwmJ2X6Ko5PC%2F7eteJ2cstmynrp1%2FUiJjoLlTOWOUrFJGxhrh4R7%2B%2FYGRbmkGrJ9M7WIUu6x5fB7G487hIBjUuFkcOlJxfG4HXBUmprfbPPSNGOu%2BSexwWmHzu6DEY%2FCFFcus%2BJ3VhuH1xl8aOWUxyg%2FVg5Bx4gU6VvXYk4mxVWqEoIuVt18pMelUqRhlNmfVA1J%2BEuVaRVlMXhy8m3BASgQrM%2FuXangLJNbadg9BJt9k34rwi9HSiJNiTBQhYyFq46Tqj1ET7fd2bdGNvfUXO1UiXpomza4ZW2Coue22a0HU%2FpBMTijYAJ%2Bf9apW8P6XwpA1clbYb5pMJXQmcYGOqUBqdVfALQD51Ur5SQMOnHFmYk5DmFQA6CNjxH%2BQFzsE%2FXl31S4MIpWpvOiP7YzmrK5dc6LotChteTqQ5ReAy%2FTb7fUyLZa7i0x%2BWIkvpyUN7SeBBJ7MBgSiThp2eExKzxynGTlelZn91s4gc7Z6yzwHoOLePaehQSeC1jd3%2F0kavV6xKScAW7uKaGOldTXizTbtMf%2F8zQsWs8l%2FAXrTaOQ5E4uiszj&X-Amz-Signature=51de4b42f436938db55477d1b04b04bd6ba6007b3f72f9d76df0b1415bf47d76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:25:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDZESLIR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T060140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIQCvq4JyN%2FxH7obRSWD6qUqW%2BwOE5b9yTyRcZUCYa%2Bj22AIgCbZSv1RAJw5s499Ktvv4teSXqoovYfMPCvuqIm2X5vIqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPooHboPAvovkYAPASrcA%2FBTD2YQSstk%2BGRVH03I%2FhvM6SL0ztfkL5TThXR6cUDHEcsj37zg05dPwcUzsAENNshJZ6LYBqsRwYwOR%2BynGBb5hFQp1GC11iP5txsdsvehQWDekC4ZJUkIKHYK2%2BsrP%2BWlEZltWAO5e9XrSzHdhhrsmE26EuYhAHxYZVdZtfSqyg9LDFdCf3GQ5UNwaSfEPCJ2HI%2FuEwew4Yi4qAqbpaAwDJJF3GKowDigWZfCYPCwFEfWQwsSdDzdfo%2BimZslzByhZ96SkfPhO7a9268QB9eujMB5%2BRBfPIioUPJJU5%2Bz7DtZtqo%2FzV6BrluG4T3GBYt7%2F0s7uMuAyAWTRbtDjOYfjDPDnN%2BBQ13w5FXaFMVKK3ubHwI6av%2FmpVYJwp3r2meEYL4j4fo8mc0rbFSGvRMj0iZa8bfim4NGtXNgsbJCG9lTfuTizj%2Fydk8W1GAXwVBWwQc0S66PN1Cx7%2FZquXC5Uq563rzxM1ZDacBa%2FSQaZHcSbUhzWiVUmxv1C5sqBhueQTGHIQLBT5mlBjVzTync%2FU87EoammUdGBdjQosjWBy2YXr5%2FAiqx7ChHXSa1GhB%2BF6UaTYYcvsgqaGu9YvJjUHtm%2F01J0subgUAE5J5D3GoTR0WufghjInIMMILSkscGOqUBp8LyOxkWoXLk8aXC8QQXzxpmtpALslBdJ1FJxerf0wzfradD5DDT5IEDQvWC9V3Wn%2FqeR4HxMBAohlvCGpdR2BL4hSl%2FKa3gpsSRB20oUuZgfoETHBEm6UCf8D1N9JGMP%2FUkx0fcn1v7%2F8IQDHrMtO8f9C6YDnylhMWtjhOUBieKrlmMIRhFDw2qRu7FaJlaNx4NTrlbm538HM96DqPKqfM2Pk6Q&X-Amz-Signature=8ebe7361be1706ba3cf3bb4d776b6389f6342538f548c3461c7dd058a426e9bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


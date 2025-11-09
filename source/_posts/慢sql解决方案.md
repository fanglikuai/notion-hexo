---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FLIQAIK%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDY0krBwkeajlAMDvWeCHfk7o6%2BTLZzk8Ezft1hiZuV9QIgYxqTrSHSALIbmxxcykdn5D8yrckc3adzw2MaBps423YqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKepHBvCHmp4k88u1SrcA1MQDLhTjdaEPpb6yH%2Bj0r%2FL8f7nRyL%2B0mWG6hvV8TtUJ9ASgLFsKTsxZ37rGPgqVzqrpUBbTGGyKQNmipuAU5EahfKha%2FQfH8kFiITuGtLWD23yo40WSB6ojjrMfX9dDOGPmBZodTztt4U0a0ltqmHK3BLJJrIQulzOEJKjiKbP%2BRAfLstI%2F%2F1AEuR636%2B7EYKfkEtLqo2NHDi%2BCnr6q0P9ZMYkr%2F1MTdX%2B0d5qqazCR7YAtEP%2Fe8IPJDqEFHnTesAvO8rghwe5hW998tkEaqwtMmiE4tr3N2XHmaFXjzZFKVlfdI916Ir3IFmZlFfylA5ZLhdnBmAL9nC%2FtZ2nWHtYrdMyEOfMDBUPACgejuhOQLGXdnJ61HWor4br%2Bh%2FQcV%2BBVjakgbaAjo8LJOxxXAmFH0nFSfuyaxj2DRCqALh8%2B7Exm%2F3TAC6SolojtuWTrBp5aHoidedVXU9cRfQdSF78OYe0Rk0fs8Zk3wLb606B8EfGAdWFArZcEm%2FOcBlXCIRdEzOjHUwZ1VcidvCOn3xI5J7xmqu%2F5dUnHL3%2FfgMhcjfe3eGPCdNYZO93si8BAQ9ZIPzTTz0A90pUaRdq%2FMnTJQlGR%2FMcqwbeMm5HTjlpCl68dRFd3wXYSVrOMOCTwcgGOqUB70Z8HG29IjnhKHv26q2CQyo7Q%2Bl5amfFq1RHww%2Foov%2FZe3eXvhRwL1YGhABy8%2FO6dKt74tTfTiAHc3H94ACWi1mdeYWTguIkawHylCD%2Bz91mKcuaikSdL1A4Jh39pGwIOETZCdguV3SzO2RLBecjmqJxQO0tlT92HRO0eueyXZGcStsAMBjKG601tkF5J%2BA7Lv5EIV6IS1TZ42yNXh7FIJrkYgGR&X-Amz-Signature=8503e636be87df7c732740657ffd22f20aef94f82377abb82f10ff4409fef5d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCRFLFO%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBRIIQN6r%2FSqng%2BbQU7F1XWnpdNTgki7w5SmUZ0fkfLBAiEApSEbwK99x8TkDi%2BjGI53apkmrMed782EaQKTzMUzYNcqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPO6SBBIKl3luQs3WircA3%2BCRJzZ%2F8Ta21EFWFPieJV0jlR47fvQzh9iY5kxDNM5i548O0EerwizVuLrpohiU3miwebhh258u21nKi0rtDWYLov4EXMAMxL0KYY7edL4wclBCjQPS0lZ6Bmh8Dkwy7gcyVmTS%2B8LY88dNHPMPeubNW0%2BlZhJwAiXoV%2BX%2B2iM%2Fl8twynfP%2BtsJ4P%2BddugwngwCyqLW9G%2BMZKB30hzrALIaaBT76aav4wooDe2IiePHepK66FzQvvSPt67zF8ilUg022X16Tlb6c5Vpj2vFsZFYNy%2BJ4wvdiW7A%2B1au%2Fj5JJPqJyFWLcLLtzjhSLF7c%2BBzljNOuC1lq3y9WvMhcqL45r0OfOwKlCY62nAhHItjThpXwIjhRWl%2BjTD8AzX7Utkx2FtcuSfzagakRU5Krm1nCxrqF1907n9eaIrBxg6XQHN82pGL7lf%2Fn1Sg%2FpT2qaBXPQrelrj4JVcKS9LAbPBmAJ6YKijIOmr51HKbghZCQDf7NQzReiGBa3zEV7yB6uOo%2B5rTJE4uaGnyRX%2FIS%2FMB6GbC%2BrIuejKSmRX0ODDlrjIh2%2By1O80tNvwjulR08MMKCqBwh6dBZLbgKY4iIPDPOwVKOiThnxPD6IqJbq1bEa%2BtL%2Fw%2BYewxoXzTMOqU98gGOqUBx6v4OLFHWHiTwsH3mwDjKJkkFQchRzzGP5pDoixg12SJkF1ZdXaIhRPVY6tRBU9TLUn7d%2Boh6C2i9cylzrjzUIYLIIIddiLR2eNxKy8p%2BuqYnKMwJryI44IalXqWjsrE58ysPa5LShhUl6C3dhxM%2B%2Ba0NjTW8XPVJBeK8qHF0srN8CMPjxSqMSf%2FdYzWDIJE3lMWJh8uOBsqBxtB9AOBEa91IL5I&X-Amz-Signature=d57bdcfbe1ff3d3abfa556c17b844569818832159a0817f4c5c67c832f37767f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


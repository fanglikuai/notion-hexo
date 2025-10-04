---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKG5TXZA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDr2a624LuCI6mpxruWwqi3Z%2F04VVIAwzbabwg%2Frjx4BQIgDunuG8zESg2gT9x4nW3jU6WITO4LLDXF4CmSmt4q3Zsq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDL9N16zzKkk186ayFSrcAxF5iSSoALQhija48FImru%2FlD7AH89i6B%2FZfL5hS20RzT5BPOxUs9NcHzniFh5PBmbYZNuZ0bUvqELid80DSknBcFtJ%2BgSjXf%2Frf%2F%2FngyTNe0T%2Fcw7u%2BLhHilW%2FUHcdiSOtlsXXqZdj5ZuRBlKticCRZ6WjxQEMrWxPWMWT5614MIF44ngDOexPFXpgm8lPFp1tPB2jjKJkXZ5IAytupyR0hPc%2BTsZgWGLTh6TnhuA2haLgHctkuMjx8PocTbe6lQs%2BunR0MNMXk7koMAkpx8WAzQwUIlk%2FPC9iFmBRm3kdWY9Bvb3wT85oGt%2BP8xbFA4lkFGfQk56QZlzqP3RDQ4O3C9BnF8NW0OyKb%2FOdfPHqTnnx9TI%2BNzEO2QpWEFgjyAlxllUCm01288JMkG2oNWCjKgrEsUDlmdrqfPCxVZZkiKeqCq8frHUgVPRMO6KAAbXna6Bt8FOd2W2GmlnwlnP6lbDuKHtK0ZeQ45zS7l7Tz%2F3SoPDc2PjYDglzp5G9E20RAHrrh8geKSeWmv9BfXIdi6smHVEBQTjWsjqRudey1KoNs53in7Sfjziu38%2BDQ8lhQ6TCvGPaoxLbhec4mCkmTyPtDIt0NdGcjjZycGrdLLWpuSFdn%2FSYbAINPMIeEhccGOqUBlZ%2Btiiwq%2BggRhlCxz%2BOgbvPZOf3tdgheDZswsJNON0HhEzkTpblxC9RNsSxLKn4PGKm5iGTvBapXio%2Fs1ooCHdo1BfVKNxFq4rv8tAZqepR699PAZXFp13lC8nt%2B58HLUGqu3GQQgzRq1QXoIX795DTUWGhcQLY%2FISxFYKAFAVWByLGGCdK4UAvcZf01u%2BvTkwXttGSqHWtb0EOxcD8Q42Zoy6Kx&X-Amz-Signature=56a5bee8b130858e5ec73c73a26a4f2afc53d842d1944be7b70110223dd31a67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


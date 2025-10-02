---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVT3CQP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZRCS2QcCibxM%2FM8TckEXgkfis%2BGOZrBTAYlWHuAiypgIgZ%2FYja4CiMTzzTBD1ncckbXairHo3ETCa8uqKI8nu%2FWUq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDI4t%2B%2BUaQQ3ZrtuzFCrcA0oqY56ZpJFzjWtQqdtgxEuCwSXEvoiJLZqlh%2BUqCe4V84CcNjoCPKI%2FYvkqtO5J6EaXiMJGFEB%2B6gCSdMkw90djCbSVlHS%2BdC4TnDUt5FYDpGhWNxvRwx9FHyraLmNdnquab1yH4LbOBxuFRqoW%2FjgGFW6LCJzbUI%2FYCPHfmgrmthTK%2BQhuRM3vHA0m6WX1uwH%2FpGoZQ5%2BI4tTOGDc40d%2FNIkcXTay%2Bx5MOYfhbBSSMK2OfQcBrJrVwqydibVKse6LBh9NYzj50mzscS0yuOnVU%2F%2F1MD8hNOLDV6%2BbhU7UuxqIu0UTQeW09Ji6z3ctV34VA8Bw6BWffEsJsGFoUqQhW%2FQT13wU23oEN6vu%2FF8EP26YK66a4G03Oz2vup7aHcpVNkSQvWHgkFaLWMYTG%2FJuzJ4sD8C1ZYlwwTOAtwGu3mALHidAW546zR1cDPidNI7AxyzDJCuIF%2BKBdCuaZXqDky8q%2BrjzDdQB7sFnzKPPhGJdUVZI20gYFdu7zlLfoprjY6%2B1MV47YW3fKfix4HV3RC7EwJRj93RPMGRSXH1%2B1T5G%2F3rhB0p7QiF48coYyEzhnIa3AMIN337vCcydyz9DbdQhEy9FugQCktqSEk6gRDis8gyieKBKeTjH%2FMMnz%2BsYGOqUBvvnzByWReODJ%2FV6PsDv6VP%2FnrLuVp2Z4VCRq%2Fq1sEVeq%2FBFkqH6lQpUtdrrweA%2FsF%2FHYxIynn4NYamcCAOm%2BrNeWN%2FF4zHDCc%2F3383DHUm%2F%2B2M%2BglEQ5oty9E8KAMfxQHgB6by0XiZIhjWd3C5Z%2BYd%2FQL2pUyZMNg%2B%2B%2FvRsZ%2FFRkBy2ObqwnHkE9uC2w5Sp3AFCZUNUQFsbbwgl7KkVVS1aiI%2FHZ&X-Amz-Signature=7b8a1dae26662d5cd1aa669181be4f3dd05fcdf042bee66bd4d61e88b4065390&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AIE2A6A%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDR%2Bi5MLWzKhPeD4gg37v50W%2B7fKFencrYqX3fgm9QkAiEAgfjGODgHgpQVWeyDpRU4BkA1kFLZ%2BSrK4phblf%2BiwRkq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDA5FuHDj%2BnGDKbSjuSrcA0Rqy0MwJh2zsNYQjjoDbYB1SBBOJ7NxTlY%2FF%2FV%2BQDfRIHR5I%2FjUKxEI8T1jo%2BbtmDBNz8xB2wkS6U8E052Y4QPorgu%2F6ZwZhs%2Bje2dPisuP7TmWcyje%2F2u4u99Cd6DXecaA1MuoUfw55R3aw43yp7sjkffUE4psITetfwoeYguFO0Y68048dvokZ4IJ9KcMbsBh8JTjyPRhhaQMrGJFOl%2BnTTvnnxaJR4Drx%2BCkPe6er5gclM6UrirBavPeY4OB3kS0lEGOOMRyQJZ8hy6MqKbG%2FZpNn1H9hwUCDCVR%2FJjMMsUvsjeCqBD8rvgbxWNGay%2Fu3giJq%2BB0jYJF8AwNUeJTDKYWfX4P3ZRewHsSdbzDEiTDe0Q%2BJYQg7GKa10rDt2JM6N4zorSZvEVaTzmzffeIPvc6NebBrud5RqiocDCOEooZD5hGJl6QFq9sdUCP4e%2FxwjAeO3FcoZiDoGa82KpJzHpRvHnZTsLDUkzB5aQEqI9UDg4dyeSt%2BdFJiBX4PLXnUFVfu%2BkN%2BNbFT%2FxVt1vhPSaJduTQWCg1sIOg0hPEm7Q6Ool28IqDYHlS9Ow7zLhkGA1cEGPhmD8m1Lj2TDntP3L5lFzNHyxyAtwkws8GmwiYOCZAEhi%2BeorFMKON%2FcYGOqUBqQ9Yol7lCF2HHW1p9VGD4SAtbrey6OG5%2ByQntaaLd86fcjpubNmykyXFLsyjlLPI3dZmdTDyT%2FStsuMGVJhzLPHfBFb6uNVNmrjbjiyLGP1QEH%2Fg2zFZnvcPZ%2Fib2ohEqmeflIQ3hhv6dcX8COJfgQ%2FKup19T6igXv8onrVdGBWBPZMsTIcQ0ZwMQpao6mxoqSaJRHQQPMNaMm40JI1%2Fn56A2tcH&X-Amz-Signature=d026930a1fac82a45d40fa05bb0d560963f0c118add82121464ff987b0552083&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


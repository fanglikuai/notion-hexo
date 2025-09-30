---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWFPDVGB%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCID7BJ%2BsQtV56q6tJ%2BepmU3gMN1iYUBHSc9tG3V5Hakf2AiEArwawzpKB0U272Ex9ItHiW9H9VgOdWWY3BYRvPuYUsu4qiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEHUlagYYm08YrOGryrcA7YKbVvu74DLkrFxwboKQ9LLcbCNNACmnwqbmV2WUy4g0ybp%2FMyYdE6%2BtWiPRI%2FiNqgSyRzgUgPC6YSnkII9ezR3vVBNe0Pze4%2BEdeVzNQzAc3pBeh7WHgBMDxveqCFT8SrRIZBwUGdF0LPQx99SZK537OBSEZ48FNvPVEkDhCFlHoEItSw1QCxl8zao20uFOE%2BHdZvBAspTyxE%2FSR963N%2BnXchNQ7%2BKjUZbUYL%2BYE4UFnU7YSjKfJchpTD4y8%2BeMpf4DuHF%2FR96QDzuvPuyM%2Bz%2BkfHYvBH5FHMzQk%2FJf4pkYEw2ly8gNtt481ZNgmzCiQjq7Bh6gOzSPFNKqIyC53hAmItQvWsq6BlG7C3p1LLCbDK2ghgvajGtqt%2FbdKeML8WLwVfeYXLkzzir5EjH2psfFWXKcWY5LiXQu%2FpoRhd%2BGA331ufzpelGaGpimwKZbd6RZXJgCN2B7r8wYdRrBiLeTLZXxXdfeWsO3g%2Fz7sZWLWN5YnQNDtfJ8bHUuGfbMvO42NJnljpE9LR8GMrOrrcTCzC%2FjPFduYxkjOvFjVN6YoZ9wbZ3Lm5BLpID8ycOW4c8lECFOnel8KRHKv4FsEMMSTSD5xfbIHwBrtrDvpWDxV%2Bck3clI7qzcUK7MIHm7cYGOqUBxKdKcKF%2BiJC8%2Bz9TGEvURCB7Bs2UUS73KHSeRaEaDU4CKGRTEUSM%2BZROfESSHSjUZFwJbP1so4SV2AHPY9sD2RfoXs6CCiYjqITP%2Fo61tJgBhKyiEafGyT5qNVGYdFnL5zHegiPIJAArUkSmN%2B6OGYFiCfkEGVGGmvnlLvEYRqwBIIhBJ%2BgLgvRo%2B5MZQkZXy5IoTb0aLsTWbJkN3hC0Q42FpVzC&X-Amz-Signature=a0bb471cdc96a57f99fc301c2e59aa22fcfbbc32e0b6baa317d158cd60747557&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


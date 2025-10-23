---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGSQNVPA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjxG6O%2BGuOoxaQy6rqN1VvbDjjkZu%2B6y2tityTQfaPrAiEA3NQdVgNEiUHOUsxRB62N7BVnAKCvkwnWVMp%2FTUXlYFIq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDOlMsv2V7rmYcWyQXSrcAzEycMsDsjeaWEqoiwOCYqfTefdoYMV%2BIjFoMqWSTjooTPWXm7l%2F%2FpQTCkv4d%2FrCt%2FpJ4dNcEsZtXAM%2BMEY4tIwniJsJugWZD8niXAwwDULd%2BJEwJ8SE0jrxGCSfEYGhWvMsa9ANFopW5WZ426zlicm9u3VWeVdlDFG7y3nCYckB8EoQA%2F6TmvdxFb2Y%2BKfdtOWj1%2BwmqZtz%2FiXi%2FEWRzIqUTErxUV9uhn0LKegyHov8C4moIIlyzuiIRiBL1GqKuf85qBdemYbxZaGhaHgsIFKxIeqNWAV7vBy1JedfhQGd9vWViCH%2FZRzNDPP8RvH8gs4MCwupS%2Fu8zO%2Bl5cs1veghgb%2FULlLqkwYZPkHmuKkiTOWu3DR%2BDaTM9WOTyzlocfkNaBg13mADonYgVa8gykx6XKHMGqcoRrQgZRluQ6B0fg9AqGNURPiO4lh0ksg41wMTl2zgH%2F1D1ovsMtbkvWGTOj5QbVPUVFk6kErI%2BQUfjBlF14xFmTWprIrklqIOJRiZkE4paQ8f4vgCRuQ8TKsjHItYqViZwIlu1NMc7sbdRqEolyZTf9pFEIzxph9q7CARlpg58I5zteq3s%2BAzkSrKwyTdtRfQi4Q9GnEOhVjVtl43zvYedTWYgK%2BZMPXI6scGOqUBz5CF1lcvgQmQaHCN5dcs%2FiR6cuEJa5hxkY3yLbYPL9vwfrZTpoZNcJunsyXaDAhK3TXUU4lmaXTLrDExJYmM5G3v7EdppxRZM4QmNOS%2FjwvY7TgeKp2gjaITgS9UsQLvMSi%2F6DiMz%2BNDizDwdgGlVdt9q4DWNFUi55YOvm8Gr1oV8kddmvEEYs%2F6uNBGnqWB9gZinldBE5mp%2FFiq%2BuvaJleq6lm9&X-Amz-Signature=4cc33d1104f2ef471c61f1e4f71beaa9c14944c36c8562e5a41b3ba1cbad21d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSIUNAOG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZxLOcBegfPJMpgeof72Fkas52stQgMoY7znJ0GKbxIAiEAoE2qrUap9%2Bm4MxaX7vfatuC%2FQwW%2BFk5uVyfphMHh3b0q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDOWiCngWLk9zlCu21SrcAzZIb7ZdylPQune4R3qPyBztXupndzxQoa%2FIj7HQSM3fZwzqm4HbwnjFbXPOK1rtE%2FDhGF7PTQ04V4m%2BNlXX93xU9ygyb9ZDOmhBZBycD5TCwd%2BTkY0USVr3rhNp6pgHQfrxuvNh6yVZGgLzo%2FPcr5fvoHqIhRJ2vq0oEZkIzUxj1vgWCVqetmGXE8bWftSyv3IeNu%2B7D85wdHuDp2jrns47KrmDmLJOJHKv9gfaX%2Bg%2FncXr%2B87CVXV1m7QhXx3RYF5XDJHzegRnANXpu8KjsGgWL6gmhk517Z78aCmZ1GK2pmsE8erHFZkWUyMiXMMo%2BVunYx%2Fu%2F0q%2BifLXAEOtdKZKH4IRRHdBa17O9L3Z%2BjBktCVi6Iq0WH%2FIA%2FFnITaMY%2BG0QJOIORmc1272QWHjnYDuxWicPiqKZmEM9Bl1XSeJsXVPkFCB1F7jjCaIBmTLEZyC%2FujdtGuqkTYjm%2BhsJtc7gADk1M73x1DPDub95pXKQpD812esfKDTh5seCR1BgxaSuUWMayPAVmg4mkDcksj94k6ruSYPQSbs%2F1SqFemuWaiHxkT5quHWoTKSFqwo0RA6eDYLw%2Fg5kaPgVDLpbB%2F028JzHdcRHo2aevtMll0jAxaUW6zvDgGFy3uXMNKbg8cGOqUB2dR9WgpWsYaFeMCDNXtJLrb25DLyz7tzJd8Gs5n4MZVIgri7bI44Vhgce9syhjnvpNMvgKKsqu9gkMAk6Q2tAue0qJ6tyZzAVZu95Zy5u8yPCdY%2F7BHf25mu14HL6liZTyO7fuHZhQYf6GWw2UBoKf%2FH45C4T097jXwur4BHvLakMfnPQ5J1MrKcHQRdhcYsb8Mzp2Qi0MMagUI3Iwycce2k4BKG&X-Amz-Signature=1d956f81c24c5b1cfb5421dec498cff566ff9248d00e1e40ae06385f2f7c2a88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


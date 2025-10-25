---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRYL7M2N%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDk2%2FgcSFCCuw2ZpuIktyKh7gL7fGt4y9nueVaHnC71wgIgMRzIJUtR49VRRjbHaMTQal3CF2cg%2B6Wl8z0jg2uXuYAq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDA1Vhp3P2NY3Qj7RDCrcA6GM4PwlHAGNxS4%2BELFBNbA2GntRZJh8ZxxHiwR4kyuvQnN4OzE%2BHZcIcsYTtGSwCIvk1z6n%2Fz6upuvPzEKsHQncI%2BwinFWNv0tdAH2sOUx6IOQkJzJpxhRhFc2PemyEy4iW7iDCpnBmF%2FEi09AIpyiA6yrcNJLgKvP2Ci8%2FjTx8Tult%2BAly9vYQW0JRh6ntLmmbd7KYXX3LCHMsGl7VLBd3XUhzJxj7st6O51f2f1RYSRmNfTw2XVoMkf2GAkr09jkLRSYQxGBJ4uWNamkNsC41NdPcB2ZgE8hQCdyoiGKr25kN8d2da4edOhlfFyc567D0rtmbo5XJD9eECL1GEeAnSh1Lgzchurj9dZBzknNcjPO9MK0kRhdd0AgwHCaZc0DM0BP%2BKoVczion4DrzkKgUdDuEcXeiAZKHJA%2BEizGe7t7cVdAjBF%2Ba3Urt5fVBVtskbJHeXT2xzY3%2BpQCTBa29I2kNfeGqEyi6vUavzHr6CcTTvpOZNg5WoQ2ObS4%2FwME653RBws04sT1QXTLAStXCb2%2BaedZ0fk%2BSVzOrVLzWytChC4vLA7mcCeWZUZxMxKAUni0SuL%2BRL9udeViEs8W6fCy02uLSaf5lfkehOrTZZkeHDL%2Fnw5JYnn3wMNPJ8ccGOqUB1z22EQZ9nMKHyuOfY7sZ8C4txL1%2F3sQWZROHBTx5Eego828W0bmR%2FLEp6z7tfVPDCBDEMVPXfiyq7ta6fyhziA%2FN7XLz7itEH6%2FxbZplMlfNctSHKSJyYAfmIdYYkJwnuaMISK8h0WCgoJpkLdqp0Qt9xtu%2FY%2FTNzLa6ookefz6%2BGUB4ASMOr7Hwbg4%2B2Gc7lHlb%2B9a8thZHcnXV8XiyEqXMnXF7&X-Amz-Signature=d57ed38e3fc4535ec7cce0d12463dc4bd37e275cb5ec6c262f2b8a7f6a9ddb88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


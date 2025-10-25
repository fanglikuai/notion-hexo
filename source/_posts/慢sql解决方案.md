---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJTWVJTW%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T030059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvXaPglCbCqrsmUvlHqe7uZVQ4B54bsBj6aHVqIrkEhgIgd4G2R1JzOFU0NEMx1R2miCJ1QoeZ3iRiBJ344gR42sQq%2FwMIbBAAGgw2Mzc0MjMxODM4MDUiDHIMq0ZxD1QU0o8MyircA4T5B30cQ4u8bp8FnLAvlsx6Y8OdY5evLGh4jlxRVlelDSidWCf3HFWGVw60P3HejrVhgRUtVWJ39RTK2vxpBIHqgppV%2BrLtmd58zhBUnBwFoNt%2BPvZ0xHMk8RNwnj4Q6CyI7zzw9LXnBl94HgaaP11eddBlkM2gTvc%2Bf9%2BTBnCJs2CfzRqapyMmh3pPe0U2nUTxnLrB4vxghyGRQbwjSqa6ZZMltpRpfgxd1Y5y5xIc7sr4CKTWHaT9KUc88gaDVnDVDfH8G1LzM4xVq7wS2NPiGsQwZzAT0O7EdK7Ngm%2F0Y4Ecmk%2BBkUElDYkfp8Hl7Y%2F7J%2BepxtLh9BpilnVixya1RTVFRL%2BjK0Gt7EKmnsOKZrNhQ2nfeDcOJ1vwKksfNuFpkDa88KX1img7jTZcoV4GWSnQHcRlbpmCbwPE7paVvRiubu7JI5mOQNFsqp3gXwLeWRd8C5N35gWdjNmrzfmbewlkAhLeStYy3qR278EdtJ4V6vl%2BjpEIociM9KRICxMo0%2Fd%2Bs6IN8CLs4Sado%2Fis%2FqHR8POOVDv1eh9yfoybTo004B%2FGXyciU%2B4r0oT4w2niNBNx48OTH3%2BZfCG8qnNX34Asbe%2B1HsDoxrG4F%2ByKWd86hFbr4h29hLANMIvu8McGOqUB8LL8E29b7yJaQ33m12f221Dm5BasJhmNIB3zrc2AGQIRh8FTbGo9kZYX0yfLC4x90Qdc0AXIhTzLUrGBs8%2BA59BHM3%2BwSL0uVTs1sTSNKPuCWmj3YHberI%2FJN3ycXLUxeJcBPu%2BOQ2BLXn3ZMf1hzeUDfh9dPcvN%2BtuzA7EvqwuLOiTexNj1EZ%2BT%2B4MnGaiNotWXr%2FSgCAj1onuIJHiZWewB0fss&X-Amz-Signature=fdedf4279de493c3d23e27eba7c6ce5febb5311afe7411fd7082493e6657f803&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


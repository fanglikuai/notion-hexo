---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W4LO2AQU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC11qxrXiH656JwrCnMfxsP2%2F6zldpsOT7gDerruejScwIgNNWrWYFhG8pLEpFNiDZfTZ%2B%2BAqKqm8GsFMngCXDZWgAq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMMAxqhLX4qqvzxo9CrcA5vzmRvPYkh9rsS5MGNKOrKLbW6c%2BKD33tjDETzNymbR6xKMKj1w%2FVnmnxokL1jxaKsb4zEEsP98XvufVTZjbPDfR%2B%2BxQ%2FlPqbh5xfbXxhDcxiE9sWSnVOdLINNolWRqrjeGC191uiczyHJKvCwLa4TSPg9yFAa3sOTd7VXP1s%2FmAURLd9cwBZFwAtLD5nAE0H7ALpKTMMAkS1f9iE7agNL42QrxDrvlGTBr9UrfwpTKbeSk15MUc82aHsZ6x8F%2Fq1Zr2%2FBdMIpdRwhBguqDDcAu91fTWHOT9HykGnoQ43TaKkgt6xDBVJjVKS7%2BtetIMdyPnhCb%2F0gohRnJyryG95JPXZam%2BeeosWLqSG8MfcaFd27jvFpiDvWI0ACjJwkOnYuiJDKwaWCi5BotfxDLX0R2b5IbUDunMGuQuB4Rsx62Y%2BCi0qhOyU9yU2Li2RCLKEKqOPvhE%2FfDWR0o4Sf90pCiXcVwDgHCmmUEafN0QOEbyJ%2BTQm560sO%2FuLrOiiCoL6%2Bq9WkLPQMxztbo4J%2FQx0yvRxy2N2%2F%2BLrKblk%2Fxh1U8I5U2DN23vaXVYw1%2FpRcacHGHhG03gY3p27fFlJsSmmoUIBpcx5fxJ%2F8J4VXN3dc63XL3J9GSlbHwnPy6MI%2FapMgGOqUBRdPTUS0MRwVRQgAJgAKjZdAcFFLsHp8ud3Qu2tJSLQcVyo28SukigjeK7bL%2BGalde2ASbH2XVxgZ4K%2FLCt7jZprYQQa7DNeF21cb9GF5OM0nGU71pkM8Ili9o%2F%2Fqz7RPmWC5Xva07XAZu5%2Bze1o0xWEoOUY2NeH8JTYaQhWWSgPVl07XMKBPj%2FYZcVZHv9xI70pOFhY4zogdHe%2BYDdg3lLO5nCtC&X-Amz-Signature=a51eef0cdf1cbf404e4e8f1fecbd6c365ac805647af5270edafdb0c7d5205c6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


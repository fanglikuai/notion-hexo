---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMNYMYRL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCICbDrVjXZF%2FfRpY1qVffhF2xxvD4yjyG9u%2FGQsnNCKPGAiAUyMuUjRBDAOuobZN%2BDvhgVMNw7%2BY6q4b6haEfBqLPmCqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQdgqgy05j11xDHtaKtwD77NyLqb74mgo3c6Y9%2FP4w3iAUUo7gjWZ8I0ZiYoKWes6TLdcNKDsNbUArHYttfwfQcUe%2FJ6XZiLG2bx3LRX7HCUredWbZYUZXABGJtH%2FVxypqnud8PulFDXOLs8IIpLC1%2FLWYHQMV5DBuPjbVkRRdfdqbqTRZGA%2FgVUNscnJDrm1CfZStHnR7N8MjJ1Yu16EgK1hwm7xF9kOT%2Fq4UA1gkyzaxU25TOkY5F1Ih6sP39j1uzHh2nZp5lJgik7RAHgECB4JgItlOorpFR5zK6vNKsuPWGOZoMqDzeJoN9pBTUkLHG%2Bc7JHFKol1Wh1YgdJ5RsSwxKR144MBZf2S5nhzovcUVX5U7PIcSWhoFgqYbsywyeXqnD9sTDyOch4Vk7LkMcoWuHNynb97%2FgE8MhghEeGmEm%2F0YSLEsjZUz2XAnsrvBpkTf3fwRafAU5uPaIGP0Kl%2Bw5v9mjs04aP%2BOzOlIpQxaaQvJHceU5cRfW1yi11A9%2BPFtGlofSnp0oeQmbptCy%2Ft%2FKw6o8CN1SxAkWx7ksgp7d3uWmgelaY90pIXQztC455ya8B3ol1wIZknDZVZcXBb1gyS3M19RSUPqIFGBW21B%2FBhepdfWh1UMB7BvPDlGKck%2FY0MbYS7ynww48%2BDyAY6pgEALf3WhNdzvALJxt3Cd%2FAg26jVb5L651Lc%2FB8Xqo3t3bKqRF0E1DfLf53DyzR15SroMulwI%2FhmV6o2qosfJ4qh8fFmqgw44Zgk791hmo92N3mxlkvZX3mwZ0R2BqhcLXIZ7yY1Te6F6DpBae6Wp5r%2BrqtO8VX9RqNnQoSpTVKdfXL9djYMozTIo1EeGwPV1su%2F%2B6oizL%2F5XMary28T6hxyBvj919cW&X-Amz-Signature=e79d362dcc26e4f2edf32df19cedcb47499014e1a7e2be9d772ff0f7a7f4d878&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


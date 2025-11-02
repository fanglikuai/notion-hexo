---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QR3XN6A%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDnVRpxS7FM2jSITo2nJ02JIfF6zwEL%2FLuXuz%2F%2FU5cnGwIgDPNgpYmG0Ne9%2FauAvV3vr6jjFsCMiTCASuEtBvq1zh4q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDAvTgw6lbyk7NDGJZircA4hwhd%2FLoC8bNR%2BVRDecskI2pCAIjpUHT03rIgkM6Nk3lb66g1NWYfX0pU2TV%2Bqi5VmOiP5hXkKE968GpKGDQ5l2ZSJmct798%2BsaJgCYGYCXBZh0NzkQqDMGB3wMRPpqL9kOlHkD0lPXXLYYFKc%2Bwa8Cl0QPF9fz5nuOf%2FzIiCPnUlj9FftueaZVCqWn04QuV%2B01s%2BB1xqTLuVKOpF2xq32Bg6970SpzU0qU3rbiy8nF7rHcudThq%2FEAh2%2FfoYTLxbvmtfqDou2c8RJhRP3dCWR5nwMxWUMYLVte3UTBRxC2njWMwGblhqmH9U5eixF%2BT0POM8pubfnU9uFDeDjcNuVAYYTcx67ToA3xijAn9nVnYLFe25vra8V3wzu9maezcCWsam8tLHm%2B6%2FeMUC%2FBahi%2FIDywwqQrMMeHm8GPaywxzrY5IbmtlH2e0S9bQPvn5CglMzNeNBDheS3od5FsvwGrwak18gc9pMcH%2BuYTS2jmxHLsIcHh68MMKQAxdHmd68aA%2FgdmPzwj8Zsz0jo%2B3%2BN91YDu%2BAOSKqpRDW2DGYfXU0kZylsnil%2BpM1sy1UD5Pjo%2BrS6UY8RyqPbdxqI%2BKf7RsO%2BhLKoWSsA4n52CbFgiB%2Bou7B%2BFmliomk%2FDML3Um8gGOqUBeTOfM6xCGfY5DhW%2BEQgLlgvFyX82e1qVT9hCZ5x3oMYbr8k3R2N8Y%2BOW9xnBfkFcnA4nbrb7fwKV0g7cGGaYugGNlFQiJjPAvzJNAxCPi1%2BZgfUsR%2BE%2Fvnac4TLRP0RLb6VQa8WDu9WsFWKigScvwHutY7pQHfpp9XIxM1vC2Tp758xLTr2DScs9RwD1qe%2B%2B2UeP0FgLZG1INs06XqGlMeUVqnMS&X-Amz-Signature=d8d0102372d5522e6f5121c906274d3daa1568e01babcd2f8f8203c25b10693b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


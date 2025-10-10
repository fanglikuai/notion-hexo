---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q72H3M3G%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIBG8o9h5wyYn0J%2BUUE7Cn9ok1f4AUc7TyF2R4u9nD4jDAiEA9%2Bdry9LzXjOrdxt%2BprgKZGigBqCS6bIh%2FMV%2FcXtz8lYqiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVN90eqhC5AP4ngxyrcA%2FBwLltON1%2FvAONSMn8vOYzl5UmFU%2BLYNyIbvvpONd3AO7fR9Udcbr4FjZmYbSxr8wpXiTkMRPGSEycalo7x7A5YF%2FnICEVlG0yFuvci%2FXRE7tIbQqow0ZRnjMeY58QOwkBpfOKx0gT48qyg4gfitVkgAwwXYaZLXLGSg9bHFEaR5Tna6wtDPNxVAaNdq3c%2FOc8%2F%2F8UCq2M16Zrqb4%2BXQe69oaWIcI92PwclgO1dyKi%2FoAWhc2gnz1wruk33QFs5tOnSeU2joyZ2T%2BsRiwpRyS%2BBpYw7rOBc%2BW8XDAiPWivBHg4%2BlsSsfi2tjQnCXz%2FJz0aZXBBn%2BU7gRbUX72EbmaNo72dSz31UaDpUb1IE%2BPQcfiedvyQBwnHSMFDhdnwT9Q%2Fxc61rfoR4Ql%2FC1XiM2owq1NTSkQiVI%2FlHPosqg5XCUL3M8pbxHWCjJf2el7LrwE1JeTFNOCErqKbzOTrWGrXw3sgiqFdBqlATFy6jE97hortj1bw2h9JKm%2FYrgMagex8CDVPba209DVMkeR0TrPKF0rY9hyK77knYSeReMHFsHqE%2FxI%2BsV5lO1zSOUueH7TyUzt1KK3rTBxdW1g1fkfFkpy7gful9hhBxw9pD0O37P87P%2BnOLbH5TzzFuMNOeo8cGOqUBHxVJe%2BloIZcVAwn0fUIlOQpYtYN6dctMAwfqK17%2B0XYTpD1N49OSTaoXNYhlBBFPfkpRJCialgG5a2Dgtp6fbNk0Cq1SdsHZSHHhsG4UGLtZOLUU0dwoownbgqE58pl7HZleuiSk2ewCWq8UsuulPudwnlJkq72N%2FJzDJMdpMSACGzIYodD2zgKlqqTquZiBBU2gd1cS71x5ByA0n3TgaWIO6jQY&X-Amz-Signature=05d206b2afdfdac7d4399ef66d85385cb60fcd20c638531359b13f620bca39f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DPAKRQA%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T160039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCcB9cd0EKnjaupmEOIxJA4uveMLjs0uZtHs6p7SYWhTwIhAJiUp5UHhxSn4yyGxRMy9%2FUwS4upnrlnNwBwy5Lv9XiMKogECPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwH%2FRzla1Ixfle0T7Mq3AOjhXRNLq9fLq5M9XjsyV6b8JdYDitDSxva1pZov%2FQgCgZD%2Bz41%2BIaTOxgAtEELNbfJyFaspSMRU0%2Bjp7wm37OPNE1AVYtmwZix927TL%2B2KDgWRDoxrj4OsmDmmGh8TxPuiEk8vxI7rs91ZDa6Jeur8HokL4etN%2F5YjQ8RRqO0xcEqqG5z4K2NZGhFSdx8tGetf6J5cLtMNKRyp4ZJER0LhufKU%2Bqmyj5%2BBbfRh2UpAV9R5Yd800Ud1xXndRtUy4X6dmURjMTXPCmBYFC7aTwHLU4e64cjC9QtQ%2BsezJvZHRDL08IX7lxLfb8togkLr4wEJ1VuB4%2F6rqMTQ6LDfWYkbAO7lch%2BEcpzTyWTbfd%2B97kSXZARKH8QccEnvPo65DolqFfnzsWBiegdveJZ5XIAGGC4Lq4vrhaki3Z3NXQ%2FoXP%2BqTxnQXRRfeFlmg1QEKtxSd5XfBCWn9%2BVIkvSwtlVxFA%2BISDc0EbtDap6xeUcx4xxvckpSyy3rX92eZWYsaI%2BZ4DQoUVMyuHUENrEZ72ZlaeGMM7QlZmZio4MT%2BntW0QlOjHQI%2ForfqEcHqMPgQ%2FnXd7otYn7LoVihLROAHe0AdM9YqjHEIRwsORhAKPUoX1DUOWHi8pJkU4O4cjCxt9nHBjqkAV7U%2BXeb7a1nhL9ZJ%2B6HQlEeJe57m64T95A07UM6Udt56GbYe%2BDwZ9SB%2BzZR6SOAinPH%2BAccr5I2K5pfcXYS0G8I7XX39vO6ySpPyj0R8SFgufXFEUGr0qKfrx8cFH4FnnsvRejIyYq3DIOMRehGAFkppgs9uFRLQv0t79KIHOlmqz69OL3JdLZ22na4tnfGph4x%2Bi4bTicPmmI%2Fozk7kCOLy%2BaY&X-Amz-Signature=dcf769a5512e611346b03389a05ecb6d3354b34033f1abbfa40446be063adba0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667D7TVIRN%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T000052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDzohNPtDtlEfEdPdDl6E%2BVFwJzj9RLSzn0XlYymvRDggIhAMYtBT8TuIpNw2lW%2BJBcuAQXQyGnnlljVy5PbZclw4NKKv8DCFAQABoMNjM3NDIzMTgzODA1IgzYgxBtpHPOLltm8d0q3AN5OAc%2FhmUSjwxJypT7mWyIl%2FxAZcr%2FmGiadVPEq0TfzZwm%2B%2Fb1Dmx5KkaLnOIWs5CbywsdKr5R1pQpQN1L9dvIZLeeW5FMYmxcqsiIEMKyeZ49Z3Q%2Fgk%2Fv3e4e934tRVjBx%2FcSCt%2FomKpO4351n8WrvRc%2BPo28SK3ulHao4Ke4Cvo4KO7WDn41jfqtNGcVLKk9crk0DSnlhEp5Cyd29yRIpUzTG25%2BxtOJrU7hV2%2FHluqEHt9MBzimiZ6f%2FKwdER%2FSGO1QDqIp6iCZ%2FymMeBgyHeuY%2FIfbAHHVkrgZCzYG3CZd97dzVve7103e4SE%2FcS5cJY7EB693FdiB8mImicOuBtkZMXhoYpdauwmrOPe9V%2FR8dZ%2Fx0%2FruZSRB3YKqWzhmemFNUJk38LNCnT5uRjKzpRp6TSzS1f7%2BNr3Ts0Hs4Bb0rOP3lnhs21TE3qeQhNArgNX2h4FUvvfWdY2PlVdvwsuoWoDJ1E4lMp6ZB5qVYv3TPz03VXb9588%2Bkn4Th4eW2%2F9GDgiXLbosU7vceKOPwnDbRy%2FtIBCQTtdJUO3v7Y6ikcOU43eOk8dnfifdoK0NnktzckDpt0GQ6Ztqz3nZoZh8v85yPxy1ZZ%2BDd3tKmVwrf57I0UUI07zZxTCN6erHBjqkAW8cOuyoSk8I8k0BqSN4GmBYXxGiP7KmSZZmNVbktNwWL4WpFm%2FTOHmnzHAVO4A4AcehAc8tz29oVmWj0QHbobRnk%2BDCfYYm%2BKpXick1EfSWkTXGOGO8%2By1nOT5%2BtU3ttwp1NYdJ%2BWDzzaLcSRugR8gK0rg3K0Su%2Fu7dPxY9x8OzoegtuYY4YVeQh%2BL%2F07T4KI1TGbFpjXOBdYA9%2FYSIcptSdNL0&X-Amz-Signature=a13db22ffa73f2413b39f8c927c511cebcb9d77667ab7054acb7b8f6bfd06c53&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


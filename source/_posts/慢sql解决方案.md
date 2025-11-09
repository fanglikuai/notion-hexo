---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KPL6U2T%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T160047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCr%2B1tqi62UTkF45fQj4cKmADta%2B5eUNxmoOzBjIVjr6gIhAId8r3dKNao5feqD24eN9JMBcW9LCNlLx7mKCLZurOTVKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9R4hi%2FVy1ykqHVE4q3AM2JPHOwYfRpgXhvEx%2Fgj2JDMVe7HLApTn4fLgn3%2F2xdqML1kYPYK6qfZ7ONxjCd%2Bv%2F2f%2FPWeM35D4uY8yqhghyCrT41INzyNZNjumzG9la8kah%2BTm7abruIDvdjOxmwt%2BRuWvoEtC7C00lqfYb9FpEj7%2BzKA4YeoLMdt%2BlBPeZ4%2BXCcXi4Y4ysH7ZrtCPus%2BYYg1TTejtFSwn6ywP9pr7hbfxrI741y7Avy%2BKkOyBPEjQylY%2BkI%2FzW6%2FUoeW3yYs%2Fu7dUEcmqk429HZkE4mWDHWo2t02nIKyMi%2F7C5XJTfPmgzlHzHgOsFDvUuUI6iwLleoUhhZdQvXaVHR6t%2B45Evk3CN%2B1onBwGc1BaZgU7gdy561UaMHMKvJm4JzgYZOewHGmw8FAMJxm1wDraG02eWn%2FmJzq2NQhA3iF60gmQhyB9NcInhTkjX56vVSaXfgL7iLInG31lvzIE%2FX2QB4L6YRTyAsSlvJnZvU6eSqUIdRBeJR7k4Rs60ukXndty3jjHAB18eaVUUaLt8NQhqjV8SwTspNtiCue22KikoRhMtoY6Pm5Y1Hu%2FLZJ5JRUZJzXzCQHnZHeM0DLJbtdXoxz%2Bnuq1WYDl4zncv2Yv%2FkFCAyoiw6BKUCpqQr%2B%2FFwjDUssLIBjqkAZplMxEZ14RV%2FsulJTL8jf%2BCTAzg0mowy99jkpiyjNNesX4byEZJ2LTIzYiH4c1y9EO1xGz2NixrG9BGi4wsxX09PWAKnFWqxjdB080v5DZmT3g5r89Jx%2F2JtBlK0ZzGwrjm%2B%2BJdKgJYdMd5Ob3MIx6WOGVr5Zen1n5OzN2r7yn%2Ft6Amwu8pH8bouwPUGaqvlIPSCMf8HzQYFdAK0cY0Yl5AO27s&X-Amz-Signature=d3be78cde88e372a808c5d9bcb3e58f3f050ee0dd43843ded593249008ef414a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFGPTL3Q%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDALyi0ZukRv3SRB%2FXyygHge9bzSHC9x25zGrMjKJJlGAiB4Rmk1aQk8yPPjNE3CVdCbh2oSv3QP4H2wxB5AgNL7iyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZySmN5hkGqKUrobvKtwDKtbQ3OsGfDWMJjoMunRwqwNR8LGrcMnAnNroIpQQ%2FMjNV9bXqOk7uk9HA0KOw7B0BVy5MXp4KQq9U0V48UcclWRD%2Bh%2BvHxn3FQUrlFPCy377TNj2mLlgIpFq6EoPiHXPeADX5lt%2BvCpPF3AVzCsyi2xFCKZrrSQQS9lK9VPjYoGoDy2AEi66rN7prQASFhvlZja%2FrjvXIQ%2FrpogFMpBpHS6ChR2AtW1DTmtQWm5KBso6DrWTmQie2R7ncI2zZS%2Bfrv7yj8EmApcM2gE6JiiO0x3eyqTL9sAMctw950gGbS4JP%2FQtReKKnJuIcLGTs%2BJ9ZjESVp88AAu%2FJxKGzO3rmpte9wLhYAWw%2FPdQTIwzQQ6WjFDllOR3aakrr5jVTp0Qa%2F5eeYkIGFT%2B0ce2oLuNCssOW1sSTdBbIkfJCjivMADLd6Wh6YbdbArwHqp7RVAjK9N9gjYbqV1m5MzhhxuLs98aq9t%2BryLVFL4%2BTH3NiD%2FVMLjP6yuJObM3cxC3b5A1BzZqt7S09tiwU5I%2Bb7vsm5I477mBrZliuq6Cfebm9RHuwOzEDqMSv5czDbaqLlZaJwpC1waXalHZB%2FnQ6AYIaRskk3J63%2FYzgf050z3tgeXl5g59aRplepUGmkAw9dqtyAY6pgH9qWLrLffTj%2FsbfkuPNdU6dULq2Y0RaCN1qTLh6J26ktIVgtlSWM4wPp%2FEq4MMb3gYWhwPzPOtm5wYy%2BAIZfqFMPCu5oaqfMVTMQNTk2aSJnlj4eyST6g2dKgZaPfOd11iI6okuekejk5q95btKTpO1CSOOi%2B198jAmIqeuz5cLs4hMyykkvU%2BKFmcSD00K0J%2BZaHUlCQ%2Fiv0MeyediYWKEFuvQXJK&X-Amz-Signature=1adaff8e76c4add8ad279a98cba95586b6dfda3746ba6d53c6f2c6e7cc7e67f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


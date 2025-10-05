---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRDGAO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T130042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9TeMLFtUL0wb9o1Rr2jK7jhqnKj00i7X7fVzQitBisAiBbaysljLw3zi711uac73HrBsqECXSaxyDwiEsMRCKRTyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMz21BkyV06jr24tcmKtwDXARca1I8lCwSq01Wip82jup79xmAWimnd1FEuoBTB0NvvjpzyuSg8w%2BJN3vYRHZhgB7JkAikp5cEGFg5mcQi4lxRxCCKi1wODRQjXl3VHO39QHpMbfN1sArmLrkiULh4lZ1OAvJFwpwt%2BcWqijziFe6sGtLECNz2FSXtZ82qFrEIRsoh89Bf16rKF%2FrOB25SU8aCv1U%2FZZp4CSWV5UWHzDklw7nlK2%2FEqBekCVuf6y9DlnEBEHFBs6%2BMAPb8%2B0ZJxoZmSQVm%2Bof9Tr1vX%2BP2qLDFFk%2BqWAAhRZNGSP2HEBAlqicPWAdic%2F7KO7dkUjFwevZsxKv7unQ%2Fu5dGUxJxPiRIDUESQldAUt8Ef32llnDXKm6LQR7MD%2FGQ47hgU57G4nzQMHEfwXtAUZEYPTs6FdSJ8Pr9PiR36s%2F%2BsAKhTpO2lIwPJ3GTdBQhCWa0ZgXsqgi%2FKD9gawREeMYc6UcQFY56mSPm%2B%2FVqHpkUs4pAZPxDS5CNnK6DxzZv6GtD0zz1c1kd5fiTbZs%2BInu6D9tYTXYo27jw%2BCIm8HDodYxNdIdtUftvtUxE%2Bsr70Hkhf06LpV09Z6zCxiDXrdJBzb2IrLQ96OfRWWXjSVKBfguv%2FWIn8ciXBWYjet6LmWYwvqKJxwY6pgEOeyg5q6g1XyXl7sf%2FK7K6hLhMVV9FkMBScbALZa4F70I20rY6GloyV%2Fqp3iS0kFpJyCU5DZGStweZblaKQ9cEB8BnOT9eRg%2FyRR34c%2F8Bqju6LReXZRiCspW7RXjw%2BHwxZsjMsfazL6omZgigGmpyeOIMWy2Qc5pJ6%2BixHUg8Dk39t7fevoYktgAugXpXiIDZpIFHH7S6HFoaNuGq9G1vkKwGjHrI&X-Amz-Signature=10377fe5a3a522cf323f5d12c0e209220db5a1169909a64f428305114f6ee764&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


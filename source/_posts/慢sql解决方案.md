---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNNB24L6%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh4iLjdZsgRMm4fBB0YgIh4SWTjZhQlOFjr3N43FC7EgIhANlwG6%2Bs8AwJ%2FsmGy9gy2M6c4fVpxBa%2F%2Fust7conLMvGKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOMFy2wJeaHmLZ4E4q3AOGRkcS2k5FZi41ygPHffSQOTP0c7t%2F9jXzFVfMYkN4evFVjOL1V%2BK%2BJK7LM14rGDntamdhJcQMgeTVsjZ2bS7UK23O7GdsRoP6O2iMBfq8ZaRf0Y5WfSGjBB5Vuf%2BhUwa8%2F6oZM5pE3FCP0eeOpPv6VC7u3IotP%2BBCw1uacBR2x5ZuaiT330FcC33PmK6IPorT3c6PZaJhrSSIqCjFud%2B8pBLxYXvdvSkZlRbPHZ9hsmm%2BWj%2F9SSS3DgaSiwPtbqhl6D5gI6Mz6h1Fh%2F2jIWl0e2VJswcibq5CzKTuoI0iVmiPKajpUODn3bsA2wpympD%2Bq67kQPtmOkUjtEczh4Lr7UIBeCq%2FA1X5lEiKYSYC22dRqHIAlD4nYWPmwteKSRqp6j4teKcm%2FgHSydxwV%2B6MynMduScKViURZxAVWAQmR70MU8xwMTiTSW7d9YM9UPTly7xeOPO3xiAX7ldn9KBCpHFzWei7c9APiNEmZG602z%2Bzwbw%2FYsMi1TeJt5AdgxAHMWvTT%2F4xvedlSxx1VeMq2D6zMHoO6GFmt01sVJr4YJgk7Ep8DhoGYcacO9EXYwv0mS6eCfFGzoIWbyP%2Bne0LSwA3HzXD0vjyOACr9WElUYpquJrpu2xN9UMusTDuq5DHBjqkAWJOH37pMELjJnANrOdtPvDvqmuh%2BbbSxFUVY8GTWY0lCR7vKB0q4szzHOm5JChmFwRloJ2mL6gjeH9UJlZW6eMsDi7iRt3WQf7WtvL%2FqvWizlpXZHm%2B4Bs6pLyWmb3JCW6QKRW9GYXxKxlZXaEMp%2BT1vzQzGzL4MvX3Lif071c5SZA6SFprGEyq0Qs8DyvMneIg03lrkzS0eTObdeet4i40v6ab&X-Amz-Signature=3553e055d7731eaf444a8aa49cd0efc852809fafd50900c38a58db454efe64be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


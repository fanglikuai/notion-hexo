---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKOEURD3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T140112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCXxvqmmLlOdzlbQOQ3iS40qNG25pSEjT7X5FeEvki%2FnQIhAIx293MsNEHbAUMN8%2BbqhPQv07G4Be7u4FGxeuim0ewJKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnaEc9OKwEEEPb9VEq3ANq%2BJObhNLa7g2TbgyeZmRKHedQl%2BkUfDQODy%2BYuYd%2BqYbCCyJ78SMUKyOy7xhb1y5J29CJIGbXcvWNpYKSRz8L%2Bf4yxG%2BUzuK1J4Lar%2F2GVE0ffODeDCXXLi%2BaKy5xxugj421tbYhpvqjbU22gq0ZVcIiSYTwKGbV10AlJc6q1ggx82qpt6Sj6%2FwzOQKmB3nNubctFaLd%2B25pp6b6Ad8R%2FjdyMZZFvv9wanaxU9D%2Bz9r5%2F1QIGYm2aKYLzoPk7ECi9t%2FMdckNnfJRUmPMHlptBeBRudsutrofRnf%2BxG3ProGdvSwkQI4wMT6yqhCYpnkMNGTcXsYPeibgwvt6%2BDGZd%2FQeI6TyN3OVJhVyuJZWnzHFkb%2BDJ4m836u5DqoiJGeEdtYUGZ0pKHL%2Ff6CrgcT8d2nlXh3JINU84Do7EqPdCi3T2RkG4X8Cp%2F9gLFUWLanab86OQXhqtxb2H6dOkElrbSxmbvQtxbHc1E%2FiyID9cE7foft1gf7CxHqGD2awsi%2Buo%2BvorzcbCD18CxJW4fWo7Wqsld8U3QborwVQ2yNe0zVKYRRQ1tUlBDIKteb5Gpd0loafnhcI921NhvJbZnBeva0nXt4jR3MrQHsjkNen4LjESp5AwtL9tfZekuDCs2Y3IBjqkAY3NWIgIHJbCtjkfreOHM52NRoRMxteYTxPRrseEF1YJXunCzkrFdh5zjFcuWSzyS6%2BIeKM2Uisn2SjcCK2S07bGntrqvdDewGJbOSDF1KafKpdifdldxSeCIs%2FQ%2F%2BRSwyErkGVeifayD3f11MKb4wWsa6LG46IGN0xlMESvTqkC9Qj%2FJhgMo6VDq0tcdl24VFiESfoB1sxRMgfA6s9XXi55STdN&X-Amz-Signature=1d336780695b2d560848d3cc7f1e3ae3286659198c80da6c7cd1ad7a6d6762de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


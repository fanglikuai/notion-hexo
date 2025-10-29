---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DP3UDH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHv1SehUpn%2FcGskqQbyKHqK1pELvH3lCzgBijIoKA4qSAiA4OBfhcoZewfOe2r2Fi%2Bc1pwG4sliiV8dM5h9yIleilSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXxhlgCO6kKJO0tx5KtwDom1TPOhXQarc4b1hRMlyrIjgAcAmg5a%2BqF8FI2eCO9Tz%2FR%2FqlTof4CTda75J8uq%2Bu58Z60lRcPyM5k6p3jMhCWswf38XV3vNvlH%2Bx%2BqfqNwfBVPbkJ7fhW2iwawCM0Hvi4QuOrFTbPsq4scEoROkYLTNLDbUyc2mQoR%2BTHNeQxL519D1bYO6LbB20hAS27x0Cf8BBLYsAZ4ViXa%2FITX4r0gGmu6pSiVc06rx34bQe9PAmac5ks2zDAw%2FhkqsVBcpn9VKmToJgRrodPQqT6cQJyHRv66hoKKRQb6I8JB%2BpNCO14I57VnA%2FeOpWevyQfuPvTPOX3tQjH9egG64QjjoVIvc1MQDCb2yLTVgK2L%2B%2FU3b3hoXabKA8lpDd0pVuWMI%2FLGQx38IiK%2BeC5gl7gb7b48Lxp0DQPGiM5dQeOsAySGKa5dq6Zdkmk6lP9DylJCvjrUOj%2FMANp50UldxWrIBqoZhlVB%2FmrEBScPwemufDQpG7ZdLLzFUjTDFjFGanJXgXLHmKE4ZWlF3ZdKdT1Vw8e9x40fwfcIyqhZpYgLenJFlxid4krMrZivrzRbtABaItel%2B0Ri%2F7CGNB%2F0Cf%2B8hEMuOK48tbtoJHsErVQhHZHrm6OtxZn9QUnAUwwsw5JuJyAY6pgFyaOf2H9dHhrAdcZaX%2FA6YfjHkYtCcd2ZmTJ8KoE4IYxy1mtPlfnEWT1A5HlNTK5qlEGJvnW7%2F93GvcLIz59CMlk7Z7AB9xgZH9qVnvF9Ea55lRtvyVleOc%2Fm3yP5tGttUCK%2BP4M1B0MII5Vp3HQQirc5wVhAwGJgmC5EjhUtIz%2F9GbpJ5WB%2BRpa4RHntOrlOB3ikQQQgxXztPMeMYbXBnh68a4Esr&X-Amz-Signature=7a7ee27b222fdc4bd912431c7459b31605cff07055dc7829701276262b86bd1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


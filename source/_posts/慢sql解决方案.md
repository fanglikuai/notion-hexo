---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634FCGF3K%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIG0qa%2Fj8XurDEA6APBBck8PqgkRwF0rf4w%2F%2BJSKYXompAiAyOlNYcVjhf9RxB20yN78ZhYwB2rbVbcStmx7jAMp4uyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjwPFYSfr0i0VmC6SKtwDxvr94FfilmJEwXqpIq%2BvOJFtVJH4qmljo5h0vtWKh0VaFtyZ1ZGx5Rl4zVv0X9r7NLT8e1brnqcTstuXMmQnpDUdVCV6zqRxLfzXwX6O2zLeU3%2BWl8HdCHmuLOpSVl3lL%2F03pEinaefl2b7qS4jOQFn1N5RqjdHVVxCNvw8OpmEhaPTBvvemprjA0PpfMqs%2FcDPTSwwpe8I4HhsxXJLeu94rtVWvHPtTH2Rh%2BoRCTCoJYp%2FBFLbT1DDabSCIfqpphhVIEeZ5kZ%2FLsDxT8UZrDFE%2BHWegyQjdtJKlih0mgb76ENqDns9EGF9up68jpkP%2B5yoYkPU%2BMZMRTcFlKnGZS67tEuRbOvj2JIp2g%2BfGJ9EWp0L7iNM5%2FlnRxnhH5mTJe5fgCuvnOZwVSu7HQpNo6JbKD34iWflCTFgGHNW6PiP5VCkUFGeI8rKT2y4yb%2Bx2yKoCfkWzUJp0x6QEL%2BTUfd5%2BAvrzAMPSu80DcKp6mZCuvBP19SWsHq8qexUPWM1I%2B5hTAqNn2y0aMqsnrSw5h%2FF0Wcq4yXJMioM%2BZ0sZfe%2Bo4Z7wjvixE8nYoiSb3WBYZ6YCCfPg36pRken0Lvyki2s48NDSFDOvaTD2rDaXZbMMcOXrXBudJ8tjXqEw%2BP2ZxwY6pgFUUxTpok%2BUj85%2BpD83aCf3DRMrJjLWB8JDbKphahWlGpX%2FbPqhjRX6DgWdBewQ%2BF5dm3KZ7kPwi4YEgRUquHscv9oN9%2Bnmr3oawcSsom2D7XT%2BZOnOgU6mGdvgBnz6xqS2i7LTTAMl6dOfvN0fMk3ZBpnrehb%2FlalDEyPa4zEMoTBmz468sNmQT%2B2YhUlA7bqo7D9NZGV2kAZGZfSNcRxHlUZRG%2Bz6&X-Amz-Signature=5fd55190950f4ded435632284ddf02043566e430943cfe72b43f93fdb10f27d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


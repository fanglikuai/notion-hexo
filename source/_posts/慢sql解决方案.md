---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XXVBYA%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAYoWf%2BF09uUeGicoqnsxRf7dthGHF3pEmcdD2nJuu6AiB6n0FK%2FHN%2F6%2BOiTxxCkB%2Fvgx6w7S%2FnQsam1MsiJlWXTyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMVockYpFEOvER1csFKtwDjxSpf9SaVsCGvaD6j8oW0cBYKYNn%2BNbddjkWR%2F51tFyLKO8H15jiCkSb%2B5UfmOXtTedIr4hOM8ot1Z96wGNRxkMDuZvQxhYQoVZSYtTN%2FtimYU9V1ezSbHJwJ46MSl80MOzCzbK3rFe433mnp22BmxF1FOQhXdZ0rnjGH3UmnZqsVkPmS7RRrCrFN870e9uEwtbZYT7XQ1RQSVCqc68EcNoB82X1UnP%2B7ai2DQhJqDzkzSfk8mNqpqAtL6QsZr4zmhxBUOD%2F1ca2U3nCTcDnh2fZZ37lUr0xHkWQm2oq5XZhRtt%2FKe9yGsDYM%2F7cTYJiqILlp6MMdSxZde%2BC0EVL7lUCFoiWOugzAaO2bNyYZsB8uSIv5VSUkLp58RIBtc9SyyTLhrDI8x6inHRSmFxTw0I4t8WHG0U9vVfHRVDMdEL46NkQve2Bwlo9m73ltsOKvzTJk2clu2zVixjJVv%2FnbiHDHHCGQPuwOkKvLsCy2mb0yMRfFHP2CBx0tieXALohxm3IkbtYMJYIUBUzTuPrkJjB33oOyTtx%2Bp%2Be6dAqREfut%2Ff5VYRa5kpKaIaOgHlVokHQJ6l8n%2BEOMwa18W2Q7xFTxB0gEzjpJTexT2aLV4lzltSZwVjNdE%2B00eowsZ%2B%2FxgY6pgGPWRM3dht2b8w8ZIWhSHg%2F5E0VINGjOjdnjCGqRU6lLM2I1frZcbpwiAkf1GdBRLmnm4c53QypWJJGITZwjQf1rnlfcJ52x%2F1ulOJH4DPPe9n1f3pqqv3aQjL2PQWW9jrUYkgDD0RqfHBoVRYWM4iQVrCY%2FVlTLHbfsUlnFcZditG7M5wTU%2BQJ3O7iudh5KYtGuQdVr0uS03Nw1zbvQ8c73%2B%2BlXrEK&X-Amz-Signature=0e45884dd9fa806a5652d9ba6f1216b7ea0deefae31c6fc5460e34e74445afc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


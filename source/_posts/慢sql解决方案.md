---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCLQWTCJ%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhvY3N2%2FIOymW2S3n9YfGslnRWjq2evBmu7oVBkcFRGAiBTt5iQuT7FLYTnJ2hq2dzXhJjWsuiwSt1dM72i0BjhtiqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMftbvw96Efqz%2B03A%2BKtwDC6VrU%2F%2BDseMEYlhkulp6BQHt9ScOF7fU4Qogpb7P%2Bu8DbdmxL%2FxtNZXvPiiZ%2FvBfXdAEfTIBgajAxF7DN9Awf%2FVZ1wTdaWuwhy%2FMuxjwD79qGDiWHzATcGZ7AxUy%2FR86%2BvJjek%2Bta4mtDsmD5w5CX4msRnBDNUW0N1d9al4mFhyJVs7a1PTp82E2yPJtkRzE1HnkDcWDovNNihQFqr4bH9cgmOyE401DiVoYG1%2BBOu8c3rMzk56qNlbOyXJYz0L0hXW9S2n0NSjUOvlK0KSim7H3%2BH0KNG1nUW5E273JJ2txUTjpStmRFRpwOhw1CGUgI5hkcOfMmkZwUh4PTjWtVrwQnkcerbAMiZXCt8OJUb%2FcTgWhz1hMyWPvJZA0Lda97VlrYgFkOm4jYL2baW44mZvQiX6PzF%2FWVno8EFZY53ErTBWSFNKO%2BWLemle6%2FT6bWKenQ%2FwT%2FJExUqBT%2B7Qrc1lan0PG19D3DmgG88Gvje0tWZ9auQfyPnfpuyYQVmFtyNZsC6CZq05sOO2ZkMmPd8xXhpYAgh4dve2aymVhlAtiyoQwyrAijqUk%2BYF0US7UiB0h%2F4t902rUQuPASbYaTem2Rv7WgvnAT6zmNnXYx0RUnIlNhR1BPaDoxtgwnpjFxwY6pgEUeKiSRRND3%2Bitvgl%2B26PyrhyA42d0JlgHVkm5WOSuzwtc1anHqIq08FkETAwnBQUE%2BtqtYlKk27SJ9%2BRN3NlwApPRnHoPgeNj4Oy4A9dsys3rDAGA1v1ooTco8sOxfV0n6okTbwvLEHoKolilpCapfk2rkepNUqOP9YpVdrZlBVSQ7b1Kut2Vc8DQHVmlovqRw%2BWDH3Z1PQhauymZo6C%2FM%2B0sQ1qj&X-Amz-Signature=e4c2b6f8eae9ff0b3f939a3b4c7750a999e44775d85100121ca7da5b946d7868&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


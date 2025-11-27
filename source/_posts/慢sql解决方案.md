---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVSZWIW7%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T220043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID78FlPBL%2BCNZT2Cq2b24csmt%2FL1CHhm4TA%2BWFVhsAyfAiEA2caWhvlKyg4HcIYMR4mqFw%2BrCNj1flSfJFwzxigdZCsqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCcI7WENX4ihWw%2F8RircA5GfIjszmnYZmLR44nP1SRq5jcmQlz62j4uT60%2BYbXPTFKj6NBROKgxV22JS2snpNR7Fa9yiuB5VVsCibir83251K6GUVh%2Br%2ByMtaXLYKwSF1XE6%2BENKDSYB%2BSglPOc4uenpNogUjFpXDNYM3mTnh5eDvmYeeV6CKoIOY6go8XwohqHmnmYO3cgU4XnC6OrYyF%2B2yIsDs%2Bc3HXB%2BZVfsT0KsrtbRA%2B%2F59cDq%2FXBona%2FHvt8qVzaNLiGvQVawpR9bkNPrtr744CONjG%2BGWobiYtyAm8Ax0e%2Flyh36IwpTuKxCtaogcohmiF%2BO1x%2Bqx%2FAeT2Q8GDnHTcpexpJLceeAjgoxHCfTCB7V992Etnh1yyjMdyiXtzGX7S%2FGlsrxQY5Uzt4FzeLSwuYtxjtwqOgAVFnrnwTRbEo09EADjr2nSC8w5jLMkVgvwna31HAiifZZO04PMvBUYW%2BRsdbUYQvq84LN2XLKIqmlu8IQM42jgXO8QZoQJ9OzFym0d7vOYjyKgeMASf3xhp5SplcIf5ZhdFLzb6LtPZVRPDPjvnmrqx8DMHuipOase9GQ2JBQPJObqER5JieomXTU8pSPgQB%2FVjd5ISw0Dnzyi7jG%2Bdpg7X9TThWJ9zlDSMsTDXU6MLW9oskGOqUBna7RwoaBjqsWbYdWyPWX5MF4PmnFNIP65RnQoFkFujzDTH2X2aqyozVqz%2Bknzh7CgzxXx4uqZKlrlMWNYuJsP86jOUjBiD4Mb4ogUf06z5E4ZPVMaQuUVBJeFZ5qt58VMcL4hjlQYjGgUi1BNHb5t%2BPYsK1uU8UGMY0MbD5mkYPkGEyk1x%2BDOLZJfx0jmk88xc716pgTMoku66Pc6c0y4R65ADnx&X-Amz-Signature=09bb0e378ca0fd59ccf12bbe73787f78761b6a5f43c7d87c3217a896c0830510&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664AD6ZWFG%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD2TXH64zO3FsLp3V9PzSDM6wl%2BrUz3ll2oEj9YBg1LiQIgW2nojr6IUzQ8oAL28Zv4dujF%2F53NzWsQ3ZaSPTZfPKsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF0fiGoJBW5c4evPYCrcA%2FxZT0Q2MaDkmgVJwsVPW3w76KF9aZFS5OGNLGWk4K%2BiirSO%2FdslKoZsanOTe2HxE0pf%2FGbePiJjjicYDckwQSgSbY%2BLc4t9uT1SW5S4uGvwhHrVXzBOkhVmkbAQdhk1YYJWkw6yB1Eyx%2F5yMgfvHpsDX7FEgKkCdyKVKj6OCf6NkI5l0zufovHenkf8W6BJLL9D8BAMb%2FAcj5iLka0Av0AoChoFHZoeWy58QyphkBknee3eeqy7iCdOpZLskp6p6fTc1jJQYc1UbfWnowTVepiBPgF7CdRaU5ER5TpmFHEsOFoLMZ3qtXljQYZoL7%2Bifw01sDVRi6VXzga4VN4%2FczrTOTseywxCsXJL21j6J9WcDRYlZ3WkknPuLCGImY9yplkVDJcLW0kBgk5p26PdGPFnqx8V4nmUAgfAmMidS%2BzyGb5ji9cQqV2uIp7Bb0J2YSAhzucZ4BmIcYsc%2FlBHtaL7qXxSjVEHVKnnNGhHwoe0ogOM9BNv5%2FjfYzdg6gQllJ510hNRcWoHCO0mFNA746T6xJF57uqcDNE0pr7qHauivsKiIoRB2kv0lOWyyQZ3%2BwGsDJVrmL9rYcVFKzB%2BGFCcZaUBMGwOADUPfgfsBYKnPdiSDYPfYTR2mTeBMLXBxscGOqUBhArngI6ixQvhOAibVngJUHyA04yxal%2Blp4xHN3Aity7GIyE9scvQABPQkhsEFZyORKRucHJgomMToSxaVwT89FPRJhUsJAD40057ySrnanm%2B38GnfK%2Bbbxci%2Byyj1sarGhhkcM524g5aygKH2adFxrzOUDtUqbckXAUnTbM0oJXwyfvCQzuXKu%2FGxQ9DKQlME1OfPyV43%2ByjjITm95rsOvadrlzJ&X-Amz-Signature=04ac8c4b647cde693cb7632aa2a6394c125bd517261a7c4cbbcc9ce02b947b90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


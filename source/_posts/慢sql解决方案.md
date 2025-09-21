---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ANXRON4%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCuXfc3%2F3Aar%2FtoT9nIhpJElIF8JpKOcDhHVEyB%2FHwrUQIgZFtWA9Ix06ajo9Jdz5AHLDJbliwdbRMa%2B%2FKjknr4G6UqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC9lFG9waWlwksVxUSrcAwhQJr%2BV%2BzY7b73vfO82FZ4RgvcW7fZJIZ%2B%2BjpaaluxNTbgqvXHdEH4waNK0baB4vJuSGR3oOji3w13%2BM26mOkJ9GoZkUtNDCcjKujDRFBAQalYLhnw7ESgmTZGGoAioiAeiIT%2FJdPKCUfqgoxTdSV9Buh6qqqYAQZ5jcnowztVsW7tl2I3Zu6FmMHT7gvO2V1sHO6RFeF5n3saUG75fbY6LGrfPbpBtRJQ9YMqQuU1wxMpSNa4AIfBmJxtOIfjUTt6CqJyRXPl%2BLhNtYifgMtSkY2z49XgHEc8z2qDJSyzSd4TBWuqQu2hiZVQx1L0e5aoSdwghzxXjlEqWXmj%2FNfxA4rhyKknmpCjquAWTs3R1aIui%2Bu5yvENh74bTDNRIl8a1FiWega5wuuc0OjweLjoEa1cYL0c02paL9NgvNEuRD1B%2BNpGZAXydNN8NOzEdKNa202HlKS33i4NjwAo8W3J7VdQeWzvaGnrQtuyP4c%2FwTonbFYnJjkeXYecg4%2FEd4%2F52llG6wdzEiYaP9jsqeO9fm0dKLt%2BRmgKJSAhOsiJGYfFk%2B1Euj1p4W3RUddlBnqUoxb1VEnaj%2FZu5mmSVgYMSmnWnOBSYtDi%2F3kwru%2FGxUtK%2BZWH1rvvBvaMtMMz%2BvcYGOqUBqWBcPHGmdbHsEkBchDqTLEpMPfUDLxe52Be27uv3m1vKa5HSBvJXDUZ7GbJ7OKyB%2BShl3qE%2By3VAJYRXLQtHNDn%2BSuairgJc5nNBfF712%2Fn1B73b37gUAriSgBLOlEQKSgX38ErD2D1ufBNRq5Lx7ugMPAs8Hn7gjoIX7Md1A2pZjq%2F6nNckgFoj93tGT0a5beYEEnNMNST%2BW7oc9DSV1pXJCw94&X-Amz-Signature=9b2c8a10d905caf616142b997d14ef6e9728a4e9a1feb41a16ec886e5cc06105&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


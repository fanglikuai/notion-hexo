---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EUMGBNQ%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbBauEZfW2l%2F%2BT4b5tpzejmfv9iE07bEH%2BtSK8sVh0fAiEA0BPyAjqo%2FS6FTVGFC01yw2eGtjt3QI8cyd%2BxkfrbLPAqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCi90KLW3cTFtkeZZyrcA99h0u%2FmykVTzZtFR4XQLlG30N5VP8mpGNy92oi0rmiJnsTciCdTd4YQw50Wl5vTwoTPqawmF71gBK7lvQ%2FA91Vkd%2Bx9bWoT4OT1qnRBHV2p4fk7HU5WRXTNVvSa8dClYYpn3ud6TDAHc3L2jZyOcLle5TgPwzDzsCWzj79DMEFkU%2BjEXdn9oNiPgcUsGmKyVrgGJaDr15c3iGBpqww0iCVsZnOkP0Ludo5yf%2FjM8no%2BUe7VsaC%2FezwP%2FJ0G9zvIzM%2FpC%2FGNXQzae4Fuak4mKINDCqAknNpMGoravUA5DnDTRaVvh%2FE%2F09RMf1d01h0HUSRyqmsdwRC55p7AOrncbu6uPTXbrewsNsXT2eQCmD2y29IsYDQONUPvyDj2IVoZ2a0rSyC%2F8gOr%2BLqRu5b44rk4JJVXZUwNI5kUkntvTfA6bk4FcU8AhVrVDTXgwVp%2FJe00FsHgyv3I0uQDnGdDu1Tx3H3LCihytwzPmW3xxVaiZh%2FqFBf0n45Nclu8H38ewez703a1zBPqRG13zOFVRlWRdf71hNfWr9yZhFsxWAe1uQyJiNLzuB0ycQE1NgB30baGS0EbrNItLbqydk7mxezplXDOL%2FRRgFzC4OvJT9BUPNxDN42c5JwB8wbNMJugs8gGOqUBIJVZtbonBgg9uzfPvk78RxUxSa0EhtHRJ5NLe0Z6avNhOkbZRN8VQhN2P1qsgUmdyj%2B92qtLI3qtEhogrXFDRksELfCza9qFWWMwUFbm%2BbanDhd4jPJxRtI7Y5Qb%2FDQ51GnfB0awks0X6Rts%2FZtwYBnDmBn75HAhvVOd5b4AWRfv4XWAjMVK8ObBPwYaO9fJlPf3fxXgi8Cur2oTYI8ceOCQw1Kz&X-Amz-Signature=89921673fc093e3bbf01c10c8f6dff4e14553091c61ac4ef2e4fe58017f5eda9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMP72RT%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCShs5XHSPE6PP8XqbF4HAX5C7HpmcsPN1axFxIDFFYzQIgDPSnLoIe7pkpokzU5EkZsuAOJUnc%2BrnIElqUu1wYNFgq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDCWhRRb8T%2BSCjSUosCrcA0KXOvSGUMga1G3lbArB1YeSW9luJ4PX1uzS8HUfVcX3fLEkqk%2BkYIeQ2CyT8AxetIYVF6ovPBOQ6aH1NIjgKrsGOeY4TAdLL5gPjD%2FAm4gG9Zx%2Bw2PJDiVHcCXbQ1xwo5zRjm4wTkTOc2ATo0O%2FwECJ4HK4p%2FLAyGLUE52bXBuoiLeT4eIxV8Ab8F6dUCkHjPo%2BdepsfGtePKIXeU9A%2BEoID%2BfRXUujh%2Fujh1E%2BYJVccwdEG%2BClcLZEFDkpqm6wuxoz0NASVB3yg3wZFbfuTUuQfFE%2B71SKSYB%2B4Ng2E7bobNkV4HRXeXZiDyF5jr%2FAtEeTjb%2BOMyO4%2FYWsTvfGnJ5xYs4WIkWRpYs%2F99a%2FdVdimwBROwdvkm7JawbCdaNdKm2nVBUzDP7Fdbe4QosMoVW4IJbFH9EpjeWYq6THH6EsbarkCFhmOVQvuzkQUqXPXLhkO66nPYBsHbg2xqtxVcAZM98rwQrrt%2FE%2FQuKFWnXB%2BVMjee3s41FbUf%2BwQqHsxNThwrsP39Pfj7jyCOwpGHgFrdMRLFdUDm2eEeUHAKpkOTeQI92zPz727JpsRisay71Ja9%2FNBKgJH2jWyqUcowv7h9I%2FFqg1HzW%2FLEHa%2BTasrWSARBSRF0EhTs94MKm2tscGOqUBlD4QF1wlwRrDgnfKKT1wEC17%2B1v%2BtXJ5omo28nUAnbyMpEMFH3WiyNAjxlDytA34WoIKsqynXV6Koj3IRbvzY6HJwtRs2VCf9NDEs8onE5A%2B%2FoyM4qEVQZeTG5yqJef2SZ8%2FQoNPv0jTTt0YbZY1g37NqG5kk7ko1O7%2FadKLkp3%2B5OsDvsFvkjKZ%2BDZrczl3GfegAhnOJ6qb0kumDvH09X0fK9XR&X-Amz-Signature=5bfa7baec0c359bc9f4e3a552d9964c5c8d37c88bac8542c9cf072ade17d15a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


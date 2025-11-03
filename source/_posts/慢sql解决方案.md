---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BBGHAOV%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKvsjjoO91M0WGo1stJeIOdh7jXR9cDB9JYGvwyxHI0AiEAgg2SRnWoZg1%2Br0%2B2rm%2BAax1FQdEhStUxCaPyk%2FhjVj8q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDJa7%2Fz3gLSE%2BIXaQsSrcAwQBsMj%2Bn1kFozASqHcefWkPC9bYtHbTvJ7fCv4%2F6t8GKsRyW7qtF5w5GsVOXIaLx9nqrXO7UdvXIG%2FSueOPiCDJSngKu%2F98%2FzI4CM%2F2ZXFV6Hh%2F5WwAzAWKTv462MxrP797L6HWzwz6aMJB0fE%2FsmUsj4gAXuqL4UDP8Mxck4JB%2F99bEIHhiJ3jSI084n0z8ubaQgxHOqDTuT6KyITCX9rmtmBc89qlRV3Gjya02Xc5XsWVOfUWCtwcyohGQ0W7D2OkUDsGawbCO%2BopuoWwzA%2FCqzS34VNb6dhoQYsDb%2B0%2F54K%2Bk7BmBS5A9sriLFz2VQCymfEFs7bjY3CJ14f2gyirsPJWcyyTJv8XHGTDzivikvtTGVf5edAX3E%2BMFKDEAcoLLbNWIjdb7x70w0EUxS9K1eMpuDvIrgOr1bH7WFdMY1zOvXOUFzaz7ih3DjFik6k5pOJa5LtaTrt6B5tOss9WeAz40HLLeUGdFN7CUbHPZUZ4qNDtW7Q%2FkBNVTDTjg4uL9c%2BxO0y%2FdGo9jwb0H2YKsTblqJgy5zefNZ%2FseImmpKUVNj8Je0bH2XKz9EC74peVpWbMoqXwDnp%2BMiqvwEuDWM9myrIemSvhVnFByCCD%2FfkBglPE8bVHYNekMNeiosgGOqUBgxOljVC%2FwFa7jF9K%2BlavnBiXtV%2FqhmtVVJhWmU5a0l4i8OcAxmGK4%2BaDEmQ4CmX3qDQW3JhAESxB37FRFo8cHAFWQXyk4PzU7v0tRD2oQdlC4fTz5RJypzjx3PU4jGr3YhKG0qIqGXsMgPM66ztPOwgalIWXo5klC0YHuJQSotQT6zscokZoxLevWy6dYCieJvZBOIlAjPprT4sOu2LByI7JsiSM&X-Amz-Signature=770876d4d288ce5cdcc95c0818776e343b159fa944734b61b43dddffe81c2cc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


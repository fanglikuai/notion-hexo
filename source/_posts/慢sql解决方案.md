---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667I64WFPZ%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIERG89VlIm%2FbUgFHj3trPdngItM821Q%2Bhn1mE%2FmOXEW7AiEA%2BtRIph3iAIFs9cwQf2w3w%2BpHJ5Do7gxKK%2Bk5aV34cIwq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPesoEnMtWc1RdawXyrcA3O26%2FCOqbET4qEf52aFkydXrIsN2Nykbz%2BR%2BIOcNMBi2TNZzcl0o1irp0GECUnN7zt1GwGm3V4kWAh9sKZT5XO5ep3xCeOPVZGIFNAGomSCI8BYp2iPP0Umjwj47u0JhMkBYM1P5QD4XDQdbdTofc3OnRAH%2BJpVF4eVy6cvzZzLLR5gRRjF01O5Rw%2FU%2BP0UtTilJ0oILYRMMwT15ngh8YYm6nLga%2BpM44IA3BYLrG9pCbCelN2Ro1ZqZ6OkhtA07ghqX5TRAEg8A4G5UwKqiZUnMU%2Bj7B8eocjikyp17kbV5SJhndcBzxuQddZRKWRzJ0VlPwcM2NAv0UUSSTVvnCXi4QNiobuRtzMxGTn7OV%2FM1dcaBe0YaYmEYQnIpGVvwU6WVWU8f6BsuEQXslPlk%2FfmchL1%2BTOPXSBz%2B%2BsaDU%2BYA2iWJ3O1f9dCd5siwZ6LkDavjqYgDqNv%2FFj9ur%2BImzKN5WuZmV3OMpWvN3lQSQSxUyTsiM93rV1J7Bh%2BCezNueAI%2BADbOFeTjk%2Bbfp2jna01%2BbaV1%2BFn9yc8FQRWjB%2F4xNcTibwxyeV3xGVM2nqpfADGyP6OihhcLbA51xoQUteoMJxlebKHv6hBzJ7XrKIXK9DoQ721fz69hljhMJq35McGOqUBjyYnkJukGB26JnnhenvkUj%2B8zNb4538tRBQl5fFoPgTzz0RfkXUChAFIZmRFAJVYrzCQ6jIFRpLb%2FxlgdaDnwgHm%2Bp29mezgMhohLm8uT2%2BOX5HQ4IZJwp%2FZof4ujOnb4uTk2sE%2FVJIWFO%2FEhlKcaN84W1SE6dEV5iXfHmznRXTrDhzGZtDhkMfrxCVT9pyqWlMWt1Uq9sJoG%2FG8DHoj5S5OVouO&X-Amz-Signature=32a14f38e89fb27e9cc5d10b18765ed1c4afe78435f6af25508602df3f3986af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


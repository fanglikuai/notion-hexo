---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BLTKDTU%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICkJFv5IdhHQBSLCNWCrnhaXHwv6K6PCWhen0ziDgyX1AiAPKS7ZpgiUfXKsE75JGfpq9VYSdVpS0nWJ7bS%2FwzBKCSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSON%2F%2F90JzEls5dxlKtwDfWIcVSjwC5y9LJHWSJAkUblWoMc3eHHXhaBiV58iyU%2BZjRQDI66GEVO2IdyNaCO84U2jxk%2F9hxMapNS56BjrGgcGuSJfjL3%2FapaGgX4qRlWz%2BJlJKwGZNcrtFsH4PQxlMFf2PtMt1yfZVQWhEgebZUtGTdx6X99BekCOjZt%2Fc3R%2B2vcBrqcM4VSPONVOyeJGvwAwLq1PjVJ35Ia31PlM5EiuxYHpCsmTmw56Y%2FN6nrv1reIa96MBxm6R2Dno2omGbrkhITqLxwjUK9YZede52Gzz3S758%2ByXBEZ9wLcCuTSlYkk0R99TPMDELfvYcOUqeWReYfHHcxzTlkXlvgrbgOp%2BalZQx3t%2FcLu%2F9Vuo%2B7nf8wF8ei0lwmTihYZoL7HOaPf4T6CGgKj%2Bfpc2RdPmtuMYoCexxUe1bZ6nBHBdZuHTtAiK3vJ8QjNRHUQEYuVqNhu95NawKZgrVymnoNL9ofXXlJbjOLqK2YjUYzQ8Df80nKstF41zyiQ1sNRSx11yhsjd5qP9nfvSbuDxZ5AJyF93GdfEUQmtPKGbBq%2BVmWIEcajGNQ%2BEkKrr1sOfZDS4%2F8wkKMizN683iR%2FrKAVAbpYq%2B0ZsjAoo30NNmDXO5v72DzocNn5JeUhsX20wmq%2BOxwY6pgHfofBHlZdVjuIwPuajKZL8rDIZ6y4l14J9cqdcwZT6s65sHywj7ZFFA1Wf1HZnaQCAD6PqbED60loz5V3xjVPDICufVtwOHJVzwDhYDBZRBv9YMUL56qSLjc%2FIQ1SOBKwSceaHKM6xTyG%2F06wCtxvUEDq3m6%2BUlKF3kXZi1t4RdQyPPLF6hsJ2EuHJbvCxsUS1YQOuDGk6tFKyX%2BK3aDrURApMXn8j&X-Amz-Signature=29d4311f83777004cc03fd5b07cfeff147f8bfeb06143a5eb7a5fcbf9a9a9b5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


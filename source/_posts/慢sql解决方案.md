---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3D2N4EQ%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD9GZ3a5WVA2vwfCn6BdJWVlIppIynTgD7%2FaDZ9%2FZV5swIgFQ9VBhQU3YrjzUZHFkAa8462xieYbKOcErmb8a6f03sq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDBKIlD6VTU4ZDPYh2SrcA65WysDtEZzZMl8mNlUBL4lrhHxbLkhEetJUcM1ij70%2FfeqxoHDS2nrXVpGwGBtUOG9m60nWxfZsdP2R9LNHg%2BT3pYY3ZDNKqILvsCfWyi0u3zvW0QFtRmebK%2BVG%2BYH8EUz6A5xyKHJ20tr7Mex1lnVlQW0JBWbWtQDLzcstg5MuSdm81VZNf%2BtSxAYJ2AjUuSwASCtF4gq%2BFTetCtgZjIl7zoHRDnuXVToY%2B51GzlHAz5niXIsGKxDPsw0AzygohrF%2Fk9do7seStczEsb1W%2FZVPLYVTDyS1UQW5ffM9Vyow5szsOneH8yS3U0QP4YQMn9iFWQiLuXk85SNlDuPCupqiIfVSx%2Bdn1DbyNc7jLcNIT2ju%2FVr7%2Bwpe%2Bp58KAN0Byc0zjuDMvTTHPGGjQx0Aj6YqHvwkrc1IVmkr1qQ6z9Yqh%2F3K2nA6BBxklZ%2FdEOTgYC6zbFimBcc0qrlzN0Pok%2B8Kg9a7Y9jBb0fRZ5ukhLIf7tJT2Oju83hUTPwpnLuJ5SZkFr7QaH%2Fv1c49Pt801qj4oRUfOnfKzLodbVX7PO0ac%2Bzc7i1gqKETtJHpUD6%2BcbotHqKcbaHpx%2B9T%2FepzSLC7d0o3eizrX3FURNuIOqOrkdEIlLW9pQX4jvEMP%2F088cGOqUBKPV28kSpSvmV3SyOtZhDEnf9zpbYTvRTENkoLCjUsx5wNewxZpYKXqyk%2FE6hYc%2Bogw1VB97Ze8MfzS%2FLPu069qrRZEB2ePzj5KKKyHlaNCABOMfA5hz54EvIEEoUnD2UJgCVAQIyCa3vIt%2BkjfEWou8AJ2B%2FglTw%2F1zWpJcPHpoIZihGHa5vhRjQDPH%2FSJIA4tHlD%2FQB0gacnW62NocbGg3V1Eqa&X-Amz-Signature=141fc18131e38ba4b05fbe056b903f2cdf439297724d6d9d8c7c9ad6d9866f93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


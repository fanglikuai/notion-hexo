---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FZ7RX6C%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIAMfjh6ohDTOk36pV03nRlX%2FVebLuvYYsyDMGDS7vNgcAiBR5XREhrljwOatsA0uBciucJGLYmSIS5X31epdoaHC8yqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3p%2Fk8au7bhh886UxKtwDH7tkjb%2BgXNBc4JDHHSdYMYtUvjnv2Y48P1OZrHgV9z32BieFZYvZQSQxwK%2F8QaxMO4nRMAH02K6qgpwIbS%2FI5kS93THlUZu28G%2F5BVgeIfHA84EI2fxA1QnQNjdDpF86H1y4ozEAD1D%2FE9F8xEcehPh7CZLKFysAVtPJPn9GhjNKKPI42nlw2dQ3FNbboCW0RhLdZfgwXvWaf4Aao773p%2FZtX6vCumKbnqNU4JHbyv4WzI76gUs7bf8gDE%2FtPBYN3LPA%2BbBZHZNdm7Pph5qJdgvzsZ75G9HiBSicX4mRc7ydc8S8VLCGMDC8zOhEFf3xsV2ZGG1zevI3tq2GIaqN%2BY09%2FgRz3AmPSdOnO1T6jWi3Xst2jXyfxwXhGEG%2FBcXDFhm%2Fm9L39EATQQn0%2BlKcYF%2BGZWZq6hltrQwvcfypjXrw0YiLqQP5mG%2FGWvZ2sMs7PQ31gWPAL4gRrUJUSNYXyd1g2EwIf1FgK3mGsweu85%2FJbilIA2yOTksnXtl7qUjyFhnHUikjEcc3U9LhQGtPgxCr11FOxD1xg5G2aHOQQPhgkdcY7dST90UZmYEIPMZm7YPflitb1GA1pqjC0dZ5walXi3Or88RKMZt%2B%2F3hqlD6SnjAeUpEp0NZXvpswprnFyAY6pgGh4%2FBBDSo1aUT75p5y8GLxsUaNvDWLf9IjyTw5cBpo8uvR5YJ02kLW9x8xtT%2FM6PWOhVTgKHtoynnJrgscXYk0DDKQRTqmC9ulAj%2F6EZwoCfwnyb1YL8%2BF2NAQ7LuiyuEE8zI6nt3CDj0Vp2jWRZ1x%2FUVsXFIy0fGjzm1qbC6Vwjru6Vl2azH%2BSZ49H4Zk7gaj%2BlZCB1hQx7%2FCzRaBOPJuaQnPFu5R&X-Amz-Signature=f1d94d0467eddeafc1aada0b42b94cdef788f553fc4e508b3e50ce22461152ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


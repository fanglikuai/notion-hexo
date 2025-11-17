---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEKPSVJ4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEji3l%2BlpL%2BXgtBO1vtn%2Fn4bhZCg4q77GSe1%2Ba5uLIDSAiAinpFiLUJJ3kY82KZy4EubyB1KuNHXSxZtCeBBjk6%2BYiqIBAio%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbXMi0I%2B6JK80p6EpKtwD4D6if%2B07bsrERf7PTWmUXEDVJ12vICyub5lJKCUCtB4ZKw240Sg4hsBmT4Uvk2Fajez8UWUf8knrmXJKxkIqnt%2BMQsKLndu4%2F%2FhwHpknt9RKpRgQ7Z0pd88FSwhpiDEXQtUtCBies1PGtqRXaiX42O1GPwcB2ABmtdiIRKSlMg7fqXRt7EXYV4SdQL95H%2BaF91J9QzAZTyx%2BcBf9omizSzWB15N%2BZBDBhPtvEFAZlV1DXZLe1ZPxpFw9IrxCsN42v2PpKVY7WJ3Q0pzPnC0yJWzYAEMLQ0CsW2Y023Ib18njZTHtJmC3Ye0X%2B1z7jBBLWeiLSJhemsSX9685i%2FIPR1TUvOTJkFcfJM4zQM5WMHeI%2FJdCOTVdasUVIw4DMb7Eticn8%2F40I%2FwlTCs6ytMXBMw3KknI%2FqZTqOtPYBOGEc80jQsetWkoRDDLrzkhra%2FRg3INznx5cxW9zExmrkjnytN%2BDcYD%2BkWbnmhbXwx8COlHDRCayxKJJd28FTu3MI7hY0Bpxqe8rtISv8jpaUtrJ16JQHpUk7Wf3ChdzzZ%2B1wEAkWpCCDLghoxRyByGZuMAaVA8PDYGYTqHPw5FpxT%2F27tMKfqUHn0QTwsH1YGZ1YGGPgjnzMwv5jBOQP8whJbryAY6pgFWUT7BmMKJ05BxjnqIsOtIUlJUCfG3fQYt9%2BGZl0%2Bb2%2BGSM9qtv5omy%2FMA3LxWXfmAno4R0DqWNOilLYAy3gPLC9SL78vSEMmLLJymEv0inT5a58cV4utoup1BiYztWh3mn%2BtVgBi93rtD6Zr%2BUuVlbHyAnXigEtIlP10U04CjPSlYCe4nVOYK01WNecx1LM22TSFuXa0MjEzKsiu%2FPxiJWX7HLAHa&X-Amz-Signature=a9fd03cd9ac2e78e9e0f65d8049b0c4e6f2b8677422565435205a98e99f99d2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


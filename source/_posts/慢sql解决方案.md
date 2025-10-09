---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUGMITRF%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDWmmMV0PkJKeRaPx7A1ZF0wMZrGLhlz5VicgOLlcQ%2BygIhAKCERDkCZDz3q841bIHDXjB8Em7%2BUJlM%2Bnr5oK6HzoJ0KogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzkxn9KYrzC7V7Mqu8q3APMC8z%2Fhd5mNt6gQHrzvyaI7gSpDPusvemoQZ7DoODE5GaXdhYZfUIctoFRrC1PhGmQe3NUbT%2F%2BxNZpdsbwFhWgjRStwWapsPoHZP7j2QhdmUdwB6utOJT0Al%2BJe8EHhF20rEI0s2HDHpbds1A2mOJ46Q%2F8LvotNY4DtZOJQGDx61jeto6jMUCFNYLvMHvgWx2JWUSKU%2F6MbbyJg67q6cE284etCAMT1p6exOiqX2EuHFhgei1o0i7JZFGe0tjqaUgkcDRuDjv%2BMzvgDeQfeM1qx7xGiAP8KGxeMr%2FbG5itbqczoinPJufXpPxqMdXm%2FtFBHq73AmTNQbzfCTIRcYcZ%2FndbdHbqAJzRF60TI6T1cmQYusWnEvdz2KnD%2BFi94%2F5PIA%2B8zoKo%2BGb6GQvs5RweRSPZ1l9laz%2Fgv4OjdaA4PQR2UDQvui%2B6pNdFhnBlmAG1YAcnoYC0jMYl70XY2wN5Yj2Lhp7rcNq2EzsW0lxeCrjHn9BR2xQToDmZxDaLb7v6OB%2BwYhDsOr4kO2UB%2FfolxJq6wNJcB16jqhZn363YbKXR%2FvwHm5XdliOYo1ox6K2ulI7MwyitmDZ%2B%2BCcLDXLUXmRC%2FfFGab7FD9HzpfqzmDLGVRmfARyh26hutTDhg6DHBjqkASxNHpFhnqbF1Fg7z1siP0cPmMlXpBB%2FGPsH3Bi9Qn8tvxtgCboibfJ0Fu3kg97tEhM51dEp4Yncf6Ufbrt1S4sGSG%2Fr0meYQFmrGxxKJzsEKqzzEGXRzW2eqxpBWKecKEkAUX9AdBvg%2FgnujT%2Bp2mj5flHDFl5w4UaOjEY9FEjrWCyE6s9AyVHVH%2BHd6yiaQBzP2sYaxWbt4JevIITStq2eAK%2FT&X-Amz-Signature=ba54ee4108dc422279aac7ffd1661325919145a2a0b86669ad0936b0d60291c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


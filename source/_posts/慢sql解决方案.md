---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IECUWPL%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvRBrlZHscJ%2F4c317DOkQvkbjzlQE4h8pDnNmxkrM7lAiA9uvdho9QCn3niFd8tfWLfRCGulpT8ghRwuugLyBittyqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlG4MjjQGJO4LGNWGKtwDnCEetrXgK3u4IBEn9bkN9E0CCI5bvhIIIajjjehp1TMtCkJOEjA4o0ZnAZmZlgZk1lRELl%2FfZPpegt9tSjMs9d5TunwIbXSGUnNcXeQnZkd2G%2FhOsHa8aqKtRHs0lkMnZBvhP04I22eSnrN%2B7pDtcc1HhVGRYpgMwwVXgNBo9Ga6EnjLU1coycWm%2Bqv1dTRj0dq9LirU1vOhJdr3fMPZO8UeXD%2B7ilqy5ZrKobscu1FZEb6PNeKTuqQ9M2VBEe2YTIdVPkFhmTbj9a6MuDLbSHPxQonMCT9oO7TlwMCYjcYevm6ZkBDn9wzKUokj4ESTW03sx75135S54GcU3WYP%2FKxTRuUJt7oUgfdYH2JSlIcjg3N799K2%2B1JwP5kcK%2FMLmv1%2B8D5XNk43rdVAO9UkkzOEJ27m%2Fo0q8jZ1Ca%2FrfAlE%2FYcH77BeDqXE58So%2FxI3%2FjxvAdBvWff9zIU2KTdkFNiDfCDqooy5S2hWS6Eh5recZ5NACm0RaRPRFnjgVocDR5Vl1vy1QBDCPMSeS5hxdXTE3ijFEckv%2FwwsZvK3CHhzH8mXFihApGLuIwu%2BthdUEA0ycqW23zEqLQTt3akWAK5IvVKf5orct6PigWsNb109v90qsCmHcv4Xi3Iw7bPCxwY6pgEA4Jcjl88BcZ%2B0PUuExWXSpRC34oKZ%2BBCAMj1sc8qTLkaF7SHcwXWNfSecl5ab%2F%2B9q45fQtGEJrn%2FFu1fjSVCu97ltAd8ksRUNP%2BdN3FI%2FYgRtpLudNe%2B%2BfssiFPl%2B2LXX%2Fm1XZfzHraejlTwfMtO4bEyQlnqedoiQ15x5%2Bg29RZhKP%2B7VROPrikrz3fEkOBiBkyfDLwtyC0mLIIHUbRfb%2BECiazKq&X-Amz-Signature=3d6a0c719aeddc3af553423e67aabc7c26d4426e2627e5780076b6fb5204beb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags: []
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QM7SNKBC%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T122855Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiJECYABtiIeEBOUowwKIHvNsm8srAKvCm7hPYuPBVIAiA7GE%2ByBq4iB5sl60I3FWv%2BvNUoc1umhXCE09XU7rYRwir%2FAwhYEAAaDDYzNzQyMzE4MzgwNSIMxzkNv8s1Klt2frj1KtwDWMCinEtVYCGIz4SBvtMNNbHcSbQz7VKW6QFcjYAxG%2FofwBeqa4%2BvsTZou%2Fyj71seNvaeC6h5DdLaAnncUvujKU7jwKMhb9rU7PmFEWMYKvOeHbiSMjY%2BB9Uc2rwT42HhAGzCM8aY28FTNgSi4%2FPWMlr0%2BJBjEn5YFdF06bOQ45%2F88nSAe0tqT6I1v58ajvBoHNi1juuXCDwIFdYmlxSEAroTbPyk%2B5CMx%2BZfrowMJU1YMMzX4i%2FiMGfp43wyg2ghSNfRqIVbq9ylv0VYRM5uMTwM%2BJvyHRHIovSEmnJU6K0B0PWunVCgyca%2Fuv7oVPHnNTIqkd12FIv%2FKb8o0WGknFgxn%2FVGZubVr1p7uwqJWTF2Jr%2FsYA%2F4OEhGteWeDE7IZ42VJOn7MxMJJ1AtmIcEbvYMDT0v4Ti4EZCIsltAEllYxfCxV%2BHURcfW8LstcU9r7zQgTufVDO7INobNVyiBp%2BRqrKUk8oIrfOIgF748SIYcarqYb3Lyct0oTCOsV2D%2FId84L6shD%2FEAUM1zvDzREPWISeUI5QjskcdEXc%2F5aypWh19bhsbBgPjP%2FWv5IiHfhjnjHnhwE83KyR%2FPYvSh%2BRKD9CYlIRW3Y6bi7JdEQsY1aNspK%2FwpYXDUKHgw%2Bs%2BZxgY6pgGNs2crdw1sr7lkBO8hm0TIFc9BwIPqgs6eRbIhjeBBDN8Po%2Byr7cLos5tQepR7OOMUyoCzPIWe%2BHCg0KczPxw3y5JdY9yq041Rii0UFZrusX%2FwrN1dXmgwo%2F32Pr6k57ohnKICYZ05ZrN%2BKnspX00A606RwEP3nWmeKxQAOI6kYD7V8qlm6UaKTagjKgl4qwMS9YQzgiMBPUp3QuqAQvepXhWCxpDC&X-Amz-Signature=17e397f4286df6e245aeeb71373fdd94f9535dca9fcfaecceb33ce2ba26348d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:25:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


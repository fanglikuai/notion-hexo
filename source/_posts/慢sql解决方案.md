---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7WVILCG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIQC3fGfHVeSZHSoxWAjuRSfgOtLgbRnJo2sVonsVheZMMgIfG%2F1%2FOkXkdDhPZhLgL%2B5jwuPEzIXpAwPWqeXSRIYwOiqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMiheRmF0Topjacdd7KtwDxd7I4FB%2BtYM0lUbjGQQzxBdVChDc3SWGGpKpfcg9DpYbARojil6goEVjnC0ebOQsXx1UHrsRyNJ5k9YSaG%2Bvh%2BdmQ2%2FwTnc6gHz86wkEm3JrCF%2FLWgghVHTGc%2B0GayzS4RgWSz5BuDE1U1jEal1nqPAFN5%2FJ21A8TEQDKZbbYRQRg%2FkYJMQ1LFG5NYLvU8NaKFvHVZWmJWUdrD7syqTi%2Bv0v8VD8p64E81g0nXSP9Xj9kiGHNYujabaOeRsIOskEnRX0QvO8LHJESGDnF1Wc0Q%2FTjiuGSdCrqadS4LWlFHzTGRgV%2Fb3E2xCpelbk%2FY6erG1ld0y9NIZKGlfGbY7LEtyFoezknnm0E4wCHvmHewPS7yKsfOyTBqaIvSlq%2F%2BFxi5DYOT67HeTQORxUboCaKCzv3ypBSN3hMdWhkvOaDhtwZdT%2F2en2i7Bbp1tel07HLfoVndxh5WDofFBiwnyIU8lfqNGjPCvaco3vFn0YtoXC2UR3h3iq7hrX%2F4E5MeZRb5Nvj4GCvkD9zcgBzArBapYPQLapU80mInNPI3%2BnnP8Yb7eF2IQF66GJJqkZE6vvHKliWKmxnr4or%2Bjg0e5mHdLGLTDJdK5dtTPFgoagMk9k13OKZszlTOAEH94wt%2B6VxwY6pgHekcD7xdtdHB8MXzsSNQwu7huFyF036W1ut%2Bxq%2Fmqdig6Uibj11iREYxM1ZBCnXC6Z1ikwZ20PQqyH%2FfyynGUr4A5LLEMDyXhw9k0jjzmOROEkS2gt1sZS0ObsqAkrQam4AXhqINieimh1W7mZptFv07ltAYgCoEgLI8%2BB%2FPCtY4ega%2BqHMfVP%2FO1pubM7KOFr03o4V0soq6JDhCouDlDzH%2B%2B4dkqF&X-Amz-Signature=d6513cadd67a6c2dedf67241bf7773cf711995763e3476175e2aa35605da37c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


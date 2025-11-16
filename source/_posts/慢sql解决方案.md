---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVNH72DJ%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T160101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCmLGrjLoCb5PAdIyTvRlUwI0qFn7LgFEDL%2BcAjyVQ4mAIhAPZeXVF9nO52bpdvtkDW7z%2BYKVWBTvzomajdDMM8YlyUKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwecammCscfy6VKCqEq3APBgeyv6V5gBjmq1C8OI81uuxtplUzGUakl7gX4D93OwRrqpr8N0oiRPAhQO%2BkVWGgK8EV%2BLtCWyNdU90Zs1hnEN5ltJpiMAMD7n8h9Xlwbq2lB2JsbW5dqw%2B0DS7NTrgAMoE8N150Cw%2FjepC7wnBNdHv0cr4L24hrRTlC9gVnzQCnwfLNnEt24hFzWk2AM9RYF2NfeBib9FrAqJfowuY6Op%2BiooU5lfY5T0KDnKpnLZ5g5Tf5O%2Bk3zpFgkKGzRn79Q2W1z3h%2F1bzmJVTIkVOsGSTkYEfCPZRk6JsUZ9giTzzGGFndbFy8SSG%2BgzDIwHMSbWwt2p06lE94RpQFph10RVj47n%2F9ZZAOTGr7EgcnQSWWcnauNkp41eLIFAZ%2B8c4%2FsXOA%2BYtJ6h0ujuWzoRPPVu3GFWtcK%2FscqVirT87qbWbpjc6V6T900i9Stb13TtRICx%2B4Srac5JsjNGXly2klFjetwAH3KPtjX6VP7tmILzwRTMZ26VDriRxO2M%2F1%2FbrImXk9SQiqyd2BAkQGEOkFZw%2Fe6hNSaT8H4Phgcgaft5WxRPa5QItamur%2FBmLlFSmES3qcNA%2BzuPT6%2ByuIdHaW9%2Bl3JMAIugkwyvT8zsPE4%2Bqg9oVw4G3HK0rA1GDC63ufIBjqkAfz%2BimWGB4AKyaOvlrWnbDp7Jl%2BQCIKwXEMEHsLIN3wKcas%2BrkxkUMYpSzO9D%2BngR%2FzVpiWe%2Bz8Mk%2FfkODDLZTQ%2F%2FXEQhZHP3VIZoSCBru34sXfE4zFM%2BoOMmS%2Fl7eHM6B%2FVJaKk0u2C%2BEpT25aoGK1H8X%2FaNiOWlQeU9F3G9z6LX5SPFkuACAArqm01UiUHJgwf%2FG6rHIdtiTI7KnrQElT7eFdG&X-Amz-Signature=58a366f9b53b43a7467b0f919184990bf580b8d71dc78ec0bc2f2ccafc041eb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


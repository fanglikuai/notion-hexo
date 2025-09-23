---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SS2PELF%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpCR17vbz0vdMdIL3At9kynbkpwrpVAys%2FEAyMPvMVyAiEA62LRU6B%2FZt0y1GHKEqpw7isnvW333ZLGikbmBxyt2CEq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDFQl%2BtfJIiE%2BA8A0eCrcAzobhb174tmYWNXLCppp5wihY0AvzN0NzvQeg%2BXQyVHhjjUjEE8sWpffGBaVRpRs%2FyP60WUQM0ynKIE%2BRH9c7Y4h4YeEkX3%2BMnRFh7prERX8f72vrcLYOyPZi5y%2BbccqKihiD46rVqZ%2F8jtxte4%2BrMuC%2FU8hPwP%2FJM3MIEHtaa6BMbKCIN5yw3QZ9d4RrJ8NiXb9NWPEBjYZ3BxMrlRQ8QIn9T2r9IfB4SCaX9wjfYufTMwJxYMGTLyOvjE1TZ4Q5jjqm6Tfj7l%2FTi3iO%2F4WVEHNaLYsy832XhvwjUVvIcz98JJgwSq71XbmUMBiVFO0DXos1uTmQb%2FsUpPGKGUP8kM5GHoUsE2M%2Bk58SqPnm3%2BBPgKyoezG6dmMVqYyv7FQMofM9JkGwfEbGYITdGpRq%2FVDd5ENyhr4Dyuyjkvy0gBMdAuxUIuBWCmTn%2BZl3S9uEjSP3ligvyg7FJMDvYFUiLVc%2FuhQX7yKvbkcaVTFh27e66P5mJVZPafVqSL0%2Bi%2BntyeUvDUu%2BvTAlwVEb0KJy4TeZAvmnQVSmXbhFsZdF4%2FNTPdLN4abz4CWhvNKy38Ph6ayqQpdKszyXQT3LBz52BZ07XrgXIAu3c%2BeJIYGVYSm8I%2FZse7JgcTxgN7mMOiyyMYGOqUBtuviNzB6Js5Ijbrpt5FD%2BwyzqvEDUxS3HoKS66vH%2Bg3z2iJsmeuJS17ud90JalX8lCrYnRoyMSCtreVE1LkFw5oymI8rNS9B9hSf%2BTI4NBd%2Fqknha%2FkTtyiaeZWbTlJGZLQmA8OcSDCcwxNEOT%2F9BZShNoHntfoBw5LHP%2BPaz9DE9VHRV%2B%2BFLFsDRpuad5yZmIiSTHNe8PYG1%2FS%2B72rGmNNLGVPn&X-Amz-Signature=28c834aac02f6cf9384f165a7c9123653306caeda15a7fe1884ffd2f01598e77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


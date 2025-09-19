---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJT7OWTO%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCZQEEWsUlnDIRSni%2FRGGcioqyEh%2F4snDnoM5I3H50wQQIgLJ7WWoD9s8mNdRFxnGEo%2F6iV%2BC6yxmWiRFdMv6X5H98qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2FMAqET%2Bj3gRJ9RVCrcAzxcHqMrXjvHXFi1tNbh1UMRBa8346zaEH%2ByGPe0isobJHELvcLobD3Psoo1MjIEXKgSwdYfcr9FtsJGTcZPzYMB87ZO288HJ62ER8O5LLisdjVDjD2mEO2nq8SdSLenHEy1PS1Fyna5DnBYDFfOm%2FeKClXsUobtkKtJO%2FPNccr27JuQa43fgIZ19Wl8xxlB54dm3bAl0GNf2RJYuVH3djclCTqRIRLf9zvjPAIir%2FWUMucDKGRWz6zZ%2FkAXNRcufRtPYNoygXUTHRgumHDvvF3JTd6hYLNeHDCjayRa5H%2FkCKneu0Vl%2FzulrHUKDCCUpMeqI0o0hVbq%2B1QsGrFa%2FyIkZkWnFn0Xh%2BJ5EikfpKoLxVEhm90Fo3jDliya3BxrDKDirxLqW85DrXoCKlMuqtyv7xmzmJAhzNz9Az1zwXkjUtYgl842jwtTXzB28EQtglSM1derW7Z74wEWQgeR8WNweU87hDhmZL8t%2FhWS5n9wJRsPi5SklOkAvHViEuiRmgnp3tFkYmI7%2BnCoM%2F0jL%2Fnz9py7%2FkhnKlsXdVbq19DZjurA36I8WHk%2FIZ0Gez4Mz%2BVi234d29kqzqxdVIlQsxUTjNDl4h0gqTauZR%2FgaEJAveAFUenHgzCLM%2B%2BvMMm5s8YGOqUBkBkIgNecbWJmWTTStw7vTTdTkP%2FU5fECv4u7Aj%2FKX1yyvhcNreb1Rqo%2Fv1WwqnfIsuBp13gk2rlt74K0M8Js97Dnf0gHHLESF%2F4rZdebLCbp6h8FCK9W9BFfLS0EEjPgm%2FM9TTdnqHEmvToDEU7sMHVJUHcycF8h68PEZYGSbtBqyN4I6BG5MuZG6R4yLeDwaxV5J9shxb6kGlKZZM3i%2BPs1uL%2Bj&X-Amz-Signature=a5c2b32305abca8a55cb6110324443b00655a061b00dc1a0061c139c5ef201ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


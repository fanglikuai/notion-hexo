---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJT5V4X6%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIDSDX8Qx0NfGlxRU9l%2FYdnHy6LJgZN5gS%2FkgT6tnDpPeAiEAno5iVC5DJhmjodEMR4tJo%2Bo8OLqdZMJl8ijLpfi61K4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNP92QMvBwKAmzpVsyrcA2kRXNuzINvsERmsmwpZwW1o34vHqjnhv3J5rNbLQsKX%2FPiAGSug3ZZsIWoLcakkh%2B6nxn1SMGevZyvDEELzvvQtQMSkyeBHqk4RMAtPaK1k3dY%2FTikGcQp5wJyXwf3VoR6NUWQbuJwGMYTW2CrLGuup5%2F2YoX6LBHLb9x5rMfmCs%2F5HvURaO3TQlVEr7xUaSEPfmitPT%2FKs5WP3VE4CwQ7eH3MPYDCIcKFgzEki%2Fm2Vz98XJGkxJhH56lI9bJ3kzBe3ARNs6ipRKSbdxrMk0oOpBtx70%2FzE%2F2DrU9WqmfC92N8jVa9CUCL5j6Tpme6BeQOYvpMfZN90y08QopKbd6GgtrOPIfQ5%2BewVieeDDN3MSs7%2FgKvjbp0uu2y99OGHvZLHCxTjOMW3QIGi4zODLu%2Ftkll%2BvOrgOf2o%2FlE9ZRMpYlKRR7Yh1IbiGzs259ihPI5P3w1tnSOnJ33ww%2FxNkkFtlQBtnCgLT%2FirkSF92Qkipv2c7By1e6c7Z6Y64g85irVbYCn2ot4%2Fu6xDjVvPPEDJPNOPTlrbbHpKjRrBLv3cq8Aoti6jAOHYCPrGUP8%2F%2BJNFo5TVjDXftmVEz31YR1fJBGJUmE1Q41EbmVCROpEJQjR%2FNnK5aXnm0DG5MIXouMYGOqUB1Lu1PRXX%2FPXeCEKLV94G7SprnmQBq5lJyy3PAtkGXsaRIfMtfoZhws1LaT9upvKGkj4v5QVVofYPwPq4xPutvtM7rCyZC7TMmJQDGeXVDSrvFHWblHpsnwkjKWNrwXPMKv0YcRAL%2FFZYUTaEjkSWDhO950%2FdeYkAv3fiGVeMWj0g82LqTwtbHX7m6loEeIDVDWw4crQFKLm4%2B%2BGkryCJS%2FsX7Tve&X-Amz-Signature=d3c08707a85adb281956ddb8c50cf9db0f5f0e1aadc2ea2ad22f2540ca07fd74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


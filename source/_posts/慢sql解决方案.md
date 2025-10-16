---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AJFUAD4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBpn8sVHqRtkyi27VehIeVyNsqon8%2BRF%2FNJuCSwDGA6vAiEAtG9E27BqdMBf28KiV6yy2NyjYuppEpJWSQ8vqIkuTYcqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJo2%2BmDLuxrezUwP%2ByrcAztLsTDOf%2Blt052FPOIoB4DqdnMfaxZ1nKYKBLIyy6G1LSrolojfe6prIrmNASDOQCuA0r1%2FKdOog2ZcDqw5YI8ZsCOb1gsWyEy0xAS4TnVHZ9Gabk%2FVrVfaP4laot9AISfEqhOm1Bkvocm%2B98ks6aBPaIK4HFEsCuREMdNIFyEA9b2iaHXMS%2BlBVyOBcw5wJn%2FSHBbW6w0XMUIgGh5ah4hqsjF71lqF3QR%2FbP%2FvmAayafIKbtxP1TF1GmHDu0yUwpCiIupPzxaiZZH2moJKdZrZoVlYTX8JiGBIAJP3cFfaT26NZvNgYRI4Oj%2FM2NiL4hvLwtBTbHPMM%2F2giD3g3g0AmSY4tJUk1A1JHf%2BlkcRo1I8Wif%2BrTgIoFjdEyUhir2thIzU4Y%2FgCZ%2Bl%2Fq8gGwPMz9yzT24cGE4k3lQvSY%2BVxkPcfqoIDrBF0aDpOxiGQOIA9Qzcwl4E6hZyvRxeairii%2FPEFHt2GhKQiIAnfTvmezs%2FoxAKJpp2nDPH4SAXg1zd%2BYF7e0JT8CZY31eLRJWKEa%2BfMAlBQRWoxJetiiUhn8b5BJ1B0tnrh38JgtcIZmfbDNuGZKlkMRGcX5QJew5Wcy96RFttHcVAzy5uqJX%2F5panaoBH05grhIOHTMI7awscGOqUB45I6ZTsH%2Fi4r359IgpTu8INNTbfCk9mtoIzYkHFH%2BuGmOVtgyLz5StgSyJtYgDyNUOdu659qV3l3yENjc%2FcbB3xCh7pioIkHOsU9F6VpbVtrQQYe6394z3E7cg91VMG5%2FOuUDwCURoMGPiETrrqjfc55MuwVR4KxoMBeDvo2xqOGlcg2i223Z54neC18JPJCWy4vz2AaouCpz%2B5AGV5TVhijPyoj&X-Amz-Signature=9c3a51daccb625082d0836c050c71808318debad6d72156b77c4651a963faec0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


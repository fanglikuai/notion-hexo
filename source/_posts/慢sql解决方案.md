---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQO4QHSK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIFnQb8oLnxhlxvkPXog7umtzy72MSTB0GWuBFcwXF36SAiEA5weTpRpvwDVl9PlU091%2FCLGjYvTD85ZrzS0jdhIk6XkqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIuQR78HjJd5FUcZbircA2EGCoPC9Z1%2BZ9JOojymoVzA%2Bmg6sjTJPR71w1miBUjACiTv3KYMaaN%2FpQM5HXeFLC8WSqui7Yt2P1VHHfBSBjONGboAmT5aWgOcpAw8oqg21GSutEKihcCzLdy0G3QQZD8fpsPQecPT2hZrizpkN0MsiGLrq22MuhJt0ifYDQGZCMikaBoz%2FR4tHye6JpzzMhvRKZCd34ASmWEbeKWImkPpQWsANkDPMeEXy%2Bll2%2Fqod8NhQKcXdJ3USSr2ZdcAo%2FpjvYnNDUJJZkEMtgYq2%2BSIV2XAOTnQUDgYreeY5riJqbvvq%2B%2Fa%2BKrKzYadZrkcR%2FNytRjt5wSTEfenbMRi7jEYToWS4ngRDif8gfRdsKt9hgBP0cwaJQuyXbbRB2gcS5vj%2BAsiiiCp8Ko6FKwwqldhNxR7DzuIlMEv24w2bPnCGgVjkT%2B%2BWyJ7%2FLaNJkT7Fn7n0ce8cK%2BA5%2Bc9NFgD%2FRFBTm5gfCJv2bp6cWPregP0OvbVyaohbAr80ZsgELwEHEKBP%2BzwVSjIX17vtEUWH%2BTcxJKi6V3Jy%2FFL8YSKTAnM%2FFXH3xx8PbUT1d2Rjx1ABSsmtyQdaOIsAycgTJw10HKI%2B8sMbAiJtPdf4JDkTgqEwivVd67vi4tuSSeUMP3Xq8YGOqUB26x1%2FUU4SPP%2BjHqmmbYyUjlFa%2BKb5tkT1VOnoJDBoL4zRDS1gNwGYgzCJKbF1IRvwH6mxo02usNplr9xEs1zdhK7SRxSVeg4imaGLNMOmDvrkThpSCNV6MTBryb36MmH851bA1fmjU%2BWw3rDEIqEPw0bL8iQ1oFiuoCeVhfNoOJoIwZPxn0dYycj43fSP8mMS6m99bfvbqWv8sllM%2FB9zLIdHHjG&X-Amz-Signature=2c1b79ca64e4933591be66c33e4f1c12c312f7ff16eb21e90b64032f19356d6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


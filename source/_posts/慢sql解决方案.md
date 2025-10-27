---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVI63XWM%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEyvupk1v8RVL7nteY1l2xe7%2BEY%2B%2BHG%2FpMY4FIHoUOryAiBpfCY%2BfihUhyZMm8ayctmX4SnlSla6bsQqc9WkD%2FDRUyqIBAid%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaxQRgVSAbeUWpNjyKtwD%2FM%2B6AUJv17ieGCf8Ldvuj2fG%2BijoEQs2ZNg2HQMFglhyKwODmPSVUoWJQ%2FasbAjTYr1V3S7t90By%2FPab6OyYaD1gwhKTTH34Jq6ddc%2FLPB0kYSxNfN0qisI%2BvzZxOrI59y%2FRbFWW77KLOIdINeDV5InCRvjIpq%2Fx4k0k3%2BSmDPGwlHA79qC73wQ7oGbP62ja%2BV3duG3v09kzRt94mn%2BmJvkF37HqDbbuyyGZTIIJ5X2VXKUBPIzGuz%2BGp27op1xmoA6sZyV3BJM7hL48pwm9Hex3NIZuxwzq7BkoydwjImDAvaakr0Yhm9Bot8BdLw3dgTePejEKeo8J%2FBGCtt%2FMyXXDnTVxSScyPUxD67V8vs4HFbgGmJpHeXZy0c45Ifkl3jQcIs7%2FdHl27SzXbchS9HDMIZ%2FI%2FRmbF%2Bo04RZM%2FkkkvJLF1fNYhEhpTqQ8wduU1vcVF1GRL%2FMcYwenMiP%2FN28l8eE%2FuJWrpIgL7%2BAJ1lSqhFCuZO3w58mJG510Zx3MupPD1Rq%2B1OLTJZ2NBO3vHjLZE21frJR%2FxMvAkThbPxxJJnW2oxdgdV8jWO3xiGh%2BzJB2Uv93R56BTD0%2BpZ3SCR3p09YYKmCxnwS4vEr5eSEd5b3cBDveTZl356ww5837xwY6pgGx3gDCD%2F0%2F4T9uIT5KD%2FViP%2Bf1xklt76Sxcbi94IoODBrfo5pNPFTQPMo7pEGE3G3d5aBWzsttAYYY0qRGv9uoZftVQbsI71SfdrUSWCtqR6%2Fqrj8qqOlzCPLTS7oJeo%2Bh4%2Ft1Nyomm3gTMEJUtqVqNkODWBliNE7dZ73%2BfOHsrWY67ILDLgLJW9rC4u0%2BALKuhpiL79mAGKGt99F0wM8FHVT%2BRzxC&X-Amz-Signature=ca33fded95f69c8be99bb1480ac0f875ab9ecae151d8d3fd8115914bf429fcfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


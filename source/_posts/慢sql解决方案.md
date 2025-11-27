---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ED5NKWY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCJxx1quKcTDqt9YoDjyQ4UOwwMVSZvMkiJmsn2TfGNwgIhAOVJJOQ3tmKSlCd3Kvomq5GUKWNdckzZxoFDM2QX7ietKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyxk1gpG7%2FbvhNJ99Uq3ANw9pPKwjrb4PSnnLdg20ScPv5Pk16ro14msSfSCVsbRph3EfOeiHyBINONCYMu%2FvJSBrhtwUJXDD%2B5eBxQrMPquWVTLVOHmxwQb7EeMB2%2B1e15LwWYVYk9oTobp15%2B5LB0fSE1zSXL%2FJo3Vxs%2FwsxTBdZ50dK2QA8OlEcdXLy110WjVN3t12RWVCGsdQgedt0VyDGC4HAm54dCgHux4r4HrgDh0iRdKM%2FRhnSkhq0xVNQGLBDCvnX5zDYBaRDLkxs6v7J35FHvHl3b60bz5RAut1u5WL9rOCKaT2%2BD8riAMjZYDZ3AcOg%2Behmv6aPoRnd7fXp0IJNJObVwKgKwLgfYB4RQYHGdTHkHhamqkaq9zipcBFC0ZlK0QpG%2FYfg%2BEM%2BlxSE7jPlelZfW10Mz9hQ3SC8LkoP%2FuFPB3Ho5joKMueWXyrXMncG2MKq8ABjlqmbYdVbYtdwsIIP3yiMYDGgArKHlMtKJjrvcTVaggkURdTLDyohB8SnpkCqkTHMQinVBZR9DaWe%2F9wZErLaNerEGuiO8%2B0R3pDkpzN3clga5Y3hoyJIM4lwIiA9lMnrmkw6zFF%2FCWhWS1mANvFsAWXqRBPxSnj2YPGGh8L75JtdARCP6lkbrS3L7Jlht1zCd15%2FJBjqkAdDeEpMT93JaPszEiG%2BKNki92K4%2B5nT716kACxLGaiYT3tFb0Zr0edQmWWT7dbkt2lSm0F9UCUA%2BnGJsgI%2BHUrt1B32Z9RJQ1qVWQeRiD3nPtBfKlJdT6Wf4EdqVDP92F3ZOFkWW45Luyl01ewyFSaQa2X6o%2F5ZLNbcSeaIXmrIbMymWnP8hfDXxIs6E4h%2B6kFW8%2Ffo3VNEUNc2oobdSbE4Vj2Rr&X-Amz-Signature=bff1766607931a7fdabb6ad565ffbe56ae6c9fc460384edc1c9e8f2ab2b9d79e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DUPKNLD%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T040056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCGHnaD6tXpxPBctFEFA9H3EKpalcofdyKKoLEnZmCzCQIhAO%2F685uAuHHBzY8CWoiS8ao9mbGqr2Q%2BUolOKNjZOaoMKogECOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8RSiGpZQaAOcWLqkq3ANPp41uob71vCJZsV5dFW2Gfv%2FQ1bASCUmFw5hcJ5NAYFtQRusr12fnn1ZPvNm53TCJ%2B4bYc0JXVM7Ut3s6a4VhxL0VimGvZmE7pMw3wp80GZikFvtmjVQVC9riFrWDaVn6mG2g7mEOGWYj4x7uYPcBkA8irAJXRcu58DF6xxHaGmLtwLYV2XT%2BK0xzEi3Ej34B%2FhJ%2BZ4l%2FbeM09iT9KRFo1Gijr96A8rHLhpbOkAnOQQrZTDwmPmuZMrrcyfP2d02TxbV6IZus5Fb3V0sVCmqv6knkgG9sL93y%2FwCK9jlegHYWLjc%2Bz0rgmXkR%2B%2FU9QCUrWSlOmWsp13zyioNiYpc1qP7vd9qZO2wjVRowmSYtYpcBtIDNInUaWsnf%2Brq2x%2BcbLpB8enHzQUGHULQjJRwH1mTomxcSCYkXVgxXVfmVPXoeuGdG8Td3%2BPzci%2FGLG2s7hCzpL9BmfY7skEgD7aFjx4hHuBxgoLGJlfCp%2Fk2rWCk0rI52JDnJg60nmhAMDKqKz0%2BnbeCPrpFHfmdKiDxGiN2ZJH1HT9YXV5CME4GbIqbTAlS0%2FWuCFVli7VFvQnwwtpIQhtRd0lY09Jozi0%2BBYLgAlC5TystnLupKFv3Wj15S%2BcFKmXj0jmeEJDCms4vIBjqkAQZwK%2FggCNnBgCY7RFzEq3kqJM5%2BrhC45H0DE4h4dtY9sOi6%2BBmZW9c7o1ksuIU%2FRtth%2F2mu8nmGu5tzl6u%2FsJNlSSB5haowl75jPMq%2F%2BXhXhFCvKM2oSvH4qvnXcnOP3aRFeVD1hljrXLBVMJ1%2F5JufwrnYUyI%2Fz41yDQCsn2wq0snH1D7Y6MtaqQi6B5fgUJ%2Beqm%2FLGgEqh55RT2IaXoYIv9%2Bx&X-Amz-Signature=d81a1bc274dc5eaad0950d8e1904344bc6feff3f08c936d3d7010d93cbbbcb43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPUOV4D5%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC5xL6r6bYPCvAU5lK5a%2BnW79smD4JG4KMzmumhdkgV6AiB6eCRT1JaWj6MSiDj1vo9JKUTeP5%2BGECsSh6o7gWNtxSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMEqQtv8vsScVKYgHBKtwDGejDMgKuRZJU6caOtDtEcheY61csW8is86Ryh8TbkINDHbaxw%2FuP1UEvXbAl7XCIIuKoLpVRYk%2BQW%2BSf4fDBPrCaU8h2Q1ts4kqGV2cVsZPQ9kShkx%2B0f%2B7qiXlPodO2vbjXY18Nl8qlLtt%2Bw77wXVJYNi3dWTxTLJ2NVMzb5k9MgEda%2BXlm9VxhX0RFsFS86%2F%2Frr7jZGWs6vnPmL2yESTZBwxJFpRB6AxVhg0UNy4B7aiLoQ%2BO%2Bkeb%2FQDxeGHwz9%2FAYISPWk4loakzBADfEScGmaOP0xgEgAkREJnlbQ4VoNfdJUkiMydmTGflOJsnDi7hY9qE3VUb1YbmWJO55j5%2FWvy7eS9tnHod1l2zgDeXVRyWMoVOs1qNtmZ6h51Vibe4p4E5EduR7huCGZQ0xOO%2BI0bY2fMhu8zP25Y5DH29TCgeqH2LOBZBbqXI2qgBUddgv%2BYx7EvNvOKkJuF1Rs3SAM9wTEbEjirXn8KbWmmzUSYX%2BZ0wuTs3fIpS9N8sRmyw1qDM%2FHe0vMR3NyQrGYs2ClQPhlXdZk14swnzQMAnLW69lh9Btm%2BndphEmV0zCl5spnNbH%2BR377v9hJsj3sHb0Z4urPPgUhvJ%2B2TrrKZEGLtjvOBOqrLCkSnswi8S7xwY6pgHvCd9LCSk7Da6dpBZQxwzbF6QbIn1WQwsK21bFSqMTHxtDQwk9kSqN9h%2FCw6JPa%2F7vugcx%2FbGVYijqttLpz9rKTKxhAvje64HxWkwhvKU8qmrMoUhzzb3NS9%2B6rBz2wlC2XIKzWeTp01Cr0wrEGylZND1pfRo4WCjsuESo91VPR1d4Jy23g1sM0EvX6ccHGGXQShe5HgfI04mZX6gRHdSZhmoKpKxL&X-Amz-Signature=f702179c89855b9c7313e351cc6a5ad949695cc41c4c9102fb3cd1860aca38f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


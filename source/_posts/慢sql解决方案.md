---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6LPP77%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T110056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFhsyikuge3MHGgVyumxwTYaxedkjIZlCPsarIKwJ75pAiBgmDr9D8Hr0F%2FARnAcPkGpRiJq763q5i4G3Gv%2BwJbFiyr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMl49xGuKI5iVoLuY5KtwDWL2D5vGGxaQCCKHv5%2BUB9lxYgECgHAqvsBISO%2FP0FjHI7354z4XjGDfuUmZut%2BNyhR%2F4SX%2BmRUOj2M9wzEkPvhxdu1QCrZeVpDEGy61po1cuRsFSezu3%2B4JB1anZ4xPqjtoPXtAsTAnyrtmPhZcKALJ2lB2fvxhAEMl%2Fny7Z1ysfRZMMchUPdkR1lXCgEvPauHDo8whM98FurJPeKEHgmlJZvz4cUBKlwtL%2B91o5gd19NCX%2FQNLKcDcp%2B526VAYFpIjJ8DG9vhXcDKJ1as9a0Fs2hlVioVrIBpdtTCYuqc8Uv%2FD5r9Lc%2FnceUhjX3NtQlQ0YQNRO%2FlIYTvNb5ehmurioyCFfaw6riPvqJ6pbxkGuh0Xd3G9hsUuawDOxN2EHdlZDEd5joIOhuvYpZNfrLGwF%2Fed0R4uHCjADkX78FNP7xNy0OhNRlA%2FNc7%2FXl3SLSI1b0iMrfHDlC5chxTGYPHMu1wJdpVSFEONB7jqywVuVow2C49PjPKdEdmkaqugiPDgzDuF6jGsjZnuv7z4u%2Fotc22QKm6ji0jlXOc%2BwtmC4HMJkRYR9ezxeMCvrYKuULwIBhVbRFwBUlDzxO8zWkEr%2BbKA42Wg%2FIFp8x5aeckRK9uz4rugf42%2F2wtswqYKIxwY6pgGLhHxakt6kSiNQii4uXsimky0z%2BZj7AYbD2Cudh%2FGkvNW2%2BvzO4qdt0v%2BeH7X1wKLhf7MGORB909uM2cJeGYfO3IXz7OM6i4OJA9orvKNOOa2NR%2FW3qFsEh2xY0u8VIMXwtErS4ADdHC4vMnwHE%2BAlVs8T%2FPCgo2m1b355Dzog8Rb5h5ih0AkjbR%2Bb975KtIcxqvys56nrdsrW7OySUTZHRVWScsLX&X-Amz-Signature=261cc490f728e1f738b1936b9ad336e47f779a619f9a602a22aab337664875cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


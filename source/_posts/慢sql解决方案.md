---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXKS2KYI%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBetCnxvmxzMm36C69fjX9557MBuSUCBcZZUCLrb4OiuAiAkbioLa%2BqoiX%2F0dKHeLTzscJ35fvGZmUzQsDRDVtjlPSr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMl7JBZsmVfBfrne8vKtwDNz3ls9Ouqr60%2B%2BMc8scTHPgLlaYqMDG0d%2B3AJkL9ZUOi0blUsumgNjL%2B%2B4mtuF1FEFuGilA03zThiyoufvjUiOJQvZSFhvdk%2FteQo6x6ip01pMEGMR%2BhMv9vsAWxYh5a8c1%2FkwliPnIDEHMuujRvG2QPFAXTNe69NtjwLsDGLf9AUmUVRqed2W4migWfxsEMwFVKLgnE8l7cXCG3sJV1QYssmw%2FEFuwY0EAsbzBpDU6pkwModyR9tf9dLlcCdQj%2FN30ur%2BIGbGXCIEwZsCjc4w24St1YA960HFJOz2zMW9dIxNf%2BR7g5S8NCzudSTopDKrnqKXWj98XMw%2BgKYeCaEKmsAj90oz%2FlpRcTlVaDNDKMXtMBfaLEq8eagvd%2BsaS%2FQx1a1iAfP24lCRnk9w%2F51aE3a9NiZzzWznUdA6IVXaN3ztEUEpddcmDnWrwltKRfqJ%2Bt1JdlIWsfOQfbn9UN6TOaaCpqhPd94UY6yErhEQPlWnMSWFzTn69KZU4C8%2FPz4Aj6GO2zaVMXNzE7JWTiw0whSVmlOFR4NBfyl4qGSGoyRbXhN%2Bb8smmNAnZPb9SISz0JHfKrPjiTTEZODJo9dgMQ48UPy1scPtG1TrsLv7i%2F8%2Fu8I3EuOQv9qfcw772eyAY6pgHj%2BN3h2Xd1pYL2AkSV0mY58MIy8zWMF0M0F1jp3jpFSYq9mbfO9ycAXlc6H%2FWE1QRHK4NpRb%2BE%2FjGz1bihc%2FvRCZ8OifFaxaV61FV2IVvBd%2FXfY%2FYBSUelnARcDEVPalQnl3Um1UoZuz0rPaD6we7i7kWKCg9zH7GQG9B6LrGlFU56y7Rt52GX%2BxZ8SK0UMC8%2B0bY3DsySNwZE0eQTLAcsHQsnyQQf&X-Amz-Signature=df175b4da2894c79a7cbd6451ccca7fb2d54c10fc9e0baf9e1895b35a16318e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WK67PVG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T200054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNuvQgpEnQEhO9oUj8fAbffVofVJElcCBjt4YWNfugrAiAX2zobOJj5BBNq8IYByWBVyY1hFJLTPvNospcQScYc7yr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMuybhBTeRftw1V3pdKtwDprN%2FxWErAzvQKkOu0LUadl0xmDuryS5N%2F8ESc2EHGhH45ZqW96Y%2FrYWF3ToxISmH0O5QyO7el1U9t%2BTO0mDTVfbUp1ISZZXq9iSeSh4tEDVkNmSqP2MlixpCRwvpWMDuezDszeJnk0lFwh1tu%2BHUzWAIdlHWSImH3nDxcpWdqdT%2Ful%2FpPc8DgS%2BgHtzvGSXZDcMTAdjiY%2BMcRBH6CKYDkN5BNZR58fRx0gue90F1zZDun3fjysS8wqQ9hSpcJwwnYDrIX9tEmcdnDBZOnVFD%2BKdA3MVtN2IsIH4%2F2g0L9Y%2BIhxuCiELZ6IRWRy1INQmSvWYcKW8AFytVDrN4XlR7TS9Da5vVXVcJrWeUGIGlpT%2FJoSCi%2FrvchZxVzrBMKFGx18m%2FaWwww%2BIpTi3Php6qTV%2BWMZXSgh559cAnZnN%2FN7y%2F%2BxLkZUe31gZuwKivNXOB09gNT10k0kF2aA6EeF2vJqTSrGkvVV0NsFIEAIDTX9FL1rpqFYPBCcrVzyrjngthPqD8n%2F3zn3Dc3ot786AX3ZMbkCpbFW%2BJphmG5bP6SSLm4x16Z82t%2FZGnK8wnORHzBRCVa%2FUr76KKL4SRo6dlcptBSyN20aJRV5vh8G%2FMdI4R1K6owWJhIOlhiAsw7P3QxgY6pgFm7LtjPeByGMm9S2rBg4vok6IoA5uiuDL8Iw%2F%2FEsQDH4qzGSfxO7pLP9tG5GRDP07ATxjGbF%2BkOVeNgfyvLB8IS%2Fb9yjB%2FNDDcPWz7CLrqJ%2BDIwmT9NZozrAhYe6KstoF%2FTsQ89IhDjXXqbu9l%2FS8gfMn0L%2FqmpZ7Oro4s%2FjyBi01qgZN6AosBFe%2B8fjnO8iaTLTHkpebhNPQ2zCGKwt24uCnr%2BeCi&X-Amz-Signature=2b2c70c352b4684134ca4e5b9d670492c37ae12370ed25bca5f0bdacebb8dd47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


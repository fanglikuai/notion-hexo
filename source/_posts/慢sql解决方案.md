---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RIM23B7Q%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEx9lC4915yce5%2Fx5Abt2nRcSqFZji1QqYb0pky40zexAiAjiNGq6Rm2Je648d2OUgu%2F32AEczZKmY%2Flr9K642ScVSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FJMWteYToHr8voCyKtwDtQDLVCZt8RyXZECfXF1louGHO8wBeeBJWpXZHIKSnZFV7e%2FPEzeTaGTXPViFcas98dUeKiuHNGRmtH50qE3b%2FD985rN8V93tDVSUAPDiVrgBamcnuMTkbjDdqTLvbJ2bl2HRUXjS6Ae7DJoFLH25n%2FH5tGOxEfen4ij1pfPe3CKEYb4fJYRJ5peL2EwcnKOj56PdV0eXRHIqrHLfNt35znRqS3BE3mEfa%2BdFgYBKsCLo3Jrr9W569BZFf7XpGKnWwzuEkpIPgl3NIGbKs0Iv7AI8eiewJbpFhMvp0yCnVUNb7RR%2FNdQxx5SeSLFYgotgyiH3aLBhYH9ILBlQv5OeJOWh4RPA4FaCMyTDrBz%2FZoQAt1YV9RqmzeFJQ67ATdxz4QhWRRSnabyU3N8RmQb%2B3pLoAHHJAwAeAljMj9SK44bU6IspH08XJ7LZ0i%2F9BJQyQN9dSG62s1C5b4n9Ouu9%2FbkDI%2B20Gm%2F7kmN0xUH1N%2FUac3GEWB9sAZXwiN1pxo1GR47RlHsAPU5I9NLZp5QQF514C75hpOeTYVUfmV2WIFtwC87BD9WKtiefNhHq0TM3VsfbQvXwgJhLEm3V7ACmm%2BGpWmMKSNGgAyan4dEtk5fAzaxz3N1%2FWmF1k9cwmt6jyQY6pgHVUV6Dvo%2BG8x0tnxoxLnATPNS2H8NSUeLn5jwde%2BkYGnpVa8NG%2FauehDj3aWIs28uxO10C05CtoNHoeZpd44eiwGtsBTBKx5uy3Wb%2BtS8OtX72%2BB60TLRtVWrng3x382UU8utUbcIMSk1UkVWdfleJLACljI82MnALIISaqYFpFrFWGEpYVro%2BU%2Fkrm0xr4E1tpsIUhSUsd9aM5WkxYMPHVQY0O4%2BX&X-Amz-Signature=857403f6b4b9129407dacae4cd9de75a4ba3a84048fc59a38934a69bd15cea3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


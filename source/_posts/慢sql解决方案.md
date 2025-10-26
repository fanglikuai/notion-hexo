---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HLOLDG6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3Kec986VIJVmtWKP5%2FkYEKiuk28MxMo6pYkyOIjACTAIgcSc8qD1g4OJga9ZGqWAkc5GtG8Agv%2F3Yt%2Fuy3SRqKmwqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBdDDazDTadm1ZES9SrcAwBan2GrlXOd8g7vjilTSi1SaHivf9rHnwNploAbB%2B%2FXHUqtaSPaLaXvBXkgU1AAgzT8FeNzpqUzLYhZWlIyaKA1sQRuK6Ty63d7kgQ3Y0pMfZ7QeOYlW6dm3tBWEHONU46U8BiNaSNZwGKovwuS6uvts2dJDmge1WF218VxWHbv8b9iO0gkZEUhiJaCi%2FSCMn59n1Piiyh1S6Wir9J0AV05Na39UJWnJqGwjKdDSm8l6Ab6T1UIexFv8XHTtvB92KqXNxB0HpKJA89%2BzDsjKpIMz25HEcdg7MSFUuNmS001bjINBRZprNHPP3sYV0DW4U6pfinhZWRLhz35tDEgT%2FYgoj7yfYizMWM6C3b6HotJ3VjbWi2MOLN%2F10xduBLal2st7Wk8FmUyh4RV8RCpkwYXw9oWBp2sY6mtxbEP6%2B1tBQYEb9c5l0IOMhP1IXfmfiYFgx1RtNwvKTeGzInUsCgoFNwGcg0DtgwMDhQMfnxa48v%2F85HZ%2BTK%2FyM8WTz5Wkp6AiLHySHrbyjzq%2FsCs%2Bobr%2FCX3lT81EW7cr5aQQ70gvGDaNM9Pw5Ns%2Bo7EBT9%2FpD%2FFsAXVpnE90zJ4qcaFZfddPC7x6EW4V7tjlGT7a%2B6Xp826sPKs1LmXneeiMIjw9ccGOqUBPb42B%2Bn4EPJvMIybp5v26UNo2uehS3w9ACOSstchM6nwlzkd%2BBRPfBwsFTE7l8pfLAbMZgOKvVOQpm0ojm2wlg0wze0ZK4Ve8WQCTiJ8vF1V%2FQHg7F00E0OiCKW8PVnIl%2FaxQLvYvEHumQc8ivGSurRFUc8Bn1Lw3eNHlK6KkhJI5PUECBUIyEbEHO8pC41lTu%2BluXX3bv6%2Btsv%2B1Bv4Hs3Lohw5&X-Amz-Signature=356cc87acf84847cd7d038a9d086079cdbae7e8a84394ee41d640977ed7dc8fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


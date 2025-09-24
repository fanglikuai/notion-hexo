---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTPFGKMA%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG4DPatU9%2F7xa6Bu893p%2B2nc0igsCJLSgj1LIVvTIrAmAiEA%2Brlymp8YihmKJ6SQ%2FzwhIujG%2Bj3SAOGXSdg5zi3MdgUq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDBhpXFNab8JcfUGiFSrcA7wy1heiiR%2BzyGZDqsCBaxCtGtVS%2FZYw44PbBEidCPzBNtXcmuOHU3jz33VmdsoOmQcaUFUJ2VFAnsS2SuS5Hg8sSSGLiSTWXjRfycXGRuckde%2BN5S%2Fkqo%2Fr15in8nCVJEezzMKPxBZQuk9v%2B7qIIMpbgCPBws5dVOfwtYuHGfM5V5sttwiCGiuXd%2BSXmuDjGbJQCmNZE2%2BBPkIAq2QN0f3uDqwp8dxUAc3RQbPXX5cZSJLLpQmjfqxJDAhKOuXt1p1H2Jwqv0UdQbjVQ%2F75p8IBN2X%2BYpVSstCJkXJ2tpQ8xT%2FOIkVcgzDSxMp2j%2FlDDxtehCQJjCP2ayRReTl3XBev%2F4t3h2zJ%2FH1gu28SwS%2FNNY65%2FmwDEpEgyeMhL61sZ04JMoDG7kaX7bdqZ5r5LTFEMXhA%2FB0c65XZ09puvCNRO%2B5YRWoW3nJ6LwxBS3um6W3dCpb%2BMwjzkBGcRI9jX6TMrukfoMkQF%2BAEQBzN2Omis6Ica4kjU3FqpI5OkdRIZ2E57aAqmQ%2F5a%2B1MNK%2B4io8vRxCitZU%2BPdzTDK7E1MBhiec2rGljzJJAOt4Q7eCJFMn%2B1BYscsRlscPn%2BN1a0VQXwBx6hCaLVzVA2GKKhR607qlq8wyCqG75El7cMI3d0MYGOqUB5kQfaaG7Nk4ULFhhip%2FhnbTsDu1D%2FoTFFFrSAJ%2FU%2F7CEcRT%2FBuYIvfVxyt0GTwvUhe1zQrwCemY%2F2ewG1lr6tOmJGHJ%2FcWy3%2FuiItwjIeSoZdAF9SY5yVlp37AuejVQ33XCmbNseahfdG6Yq7uz9lQ%2FOkzYQjaAXLou1Va4j27uMUL2OjSbUNeMpHVxMMpZP%2FrvNTSa0eHwcB9pAlRfDXVhlTY7a&X-Amz-Signature=73425a2e7f84f79dc2492ee12d187bebc766b1cd0e3b57be28e2bf08a20cfcd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WIGTJBY%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQCJH8xTjt%2FR0DqKW1iBOH6f8p4VrGDXiSBor0tr2bUOpQIhAObNCB3fZkPA4PcAAwVJdpABGFIBPTVUNvwZ%2FQDN0qyRKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzVcpxkMXJeWnuImT8q3APTtZF3wfNbfyJsezc9Z5JYORXrmA2RL%2BJxEQtjj20DfRa%2Bu%2BvSxV29WCFVt%2F8uaG1PP5bTk8d8Yt8wTUwwaK6Vdc8IU8T5wJYG7NimUdnA7a%2F75b1MSjfF5syneh9zsNkO4OQDy0XXGbAxeH%2FfTF29rxEMxrgMhPx8nhsjC76lpGFXzts6ZUcP2x%2FkpMpcbqlv%2FcbuZ%2BdW%2BwuxdexIMNTG39S0ugTg3bdjB2HQM8KUDUqoDgZgHJhamTXtnMvdh2nd%2BFNr7pE2MKDmCi6L1jIraTEP00DW7oHPP7wi%2F14Ul88y3Xpq7HCTj1cb7nGD%2FLWcNGjDoypngkwFY1JDaMvuexEDKpELkhZhOP9%2B0do2a7XtQXyl7oI%2FVTajNXqcr5pr4d%2BWn%2BE%2FKsy5L%2Bl5o%2BWtY7AaYGCzWKw%2FyIU%2B9GA6JCxs9jb7gf%2BbCJVj1CLJ2o4owFYVTgQVKKGQkphS3vxC877MuYaU%2FROd3sK8r8zI9egQMK%2F85z3YVanqOu4ZVfVrG0bAvDvrr%2FiRoYj676hSg2id5CxWwV1I8OOORxkG0IQv%2FY11qT8%2BIuE%2Br5hOjT0IFROU0tW3riagGIf%2BDA0%2FIdKwqkXpRxFDl0TJM8vQrYrIFLiql0trJ%2F2LDTDf36XHBjqkAWhEGmuTkX2jYsKqGyrFC3dW135uC5LfJ9%2FcGBU2Lm8QXykk5tPCrOC2BRnL5umT2hN%2BUt7GCGM4lD5q9kLY1A9jAVVICkRMpHyYh%2FLaaS1JJBrSx5T%2BQAA8rWZL8%2FRFkYRrLdTvHz1Knljn%2FS07Uz1yPhtQ66FjSJQBocSswILsubuiifkpFMlFxlycAfpf40iWshBh%2By7e2NeEaR19%2BMfoNVDe&X-Amz-Signature=db6bddac54ac3c09688b8ffff7d493a53fda2f8d99135382ee327a97c717de4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


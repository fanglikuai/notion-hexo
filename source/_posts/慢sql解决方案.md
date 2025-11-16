---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQ42PURU%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCyff120Hs7fsVJVX8gsUf9ohDCtIpYvolY7LkSNHayMgIhAMBHpMSn2zxp5MsCeXPq7fV4Wqxainp%2Bga0ckfb17v6ZKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwlco0gkYHDR%2BSBcRwq3ANfhg%2B9tF0iUBmKD7B5Ml94lX9vxwjBFWB%2BRDHxKH%2FNU9pWUsJdSUrJyKOnLGEoRUHmeGEEfk%2BttmVG0A7f6QZgB2gwhw4NrHuR0aQEBV%2B4oLPMUT3qU%2BDGlx4X6HRMsvu2RWISzRNjf3Bo08e8HZuKMUgdabM0IaYfG9ObwMX30LgceoMzi8VulTCToBkMwZlPQN807Bzl5AMgiTMbmMg4wbGlq%2Be1PXecrqharQuGn8T0DDQkEcemqUX022qLtilXWyHVnkIPuCJ4aDUUk9Tq%2FVmyUZKb%2FUKojomLjiJI6G8GNpFR5EO%2FtCSRGQCenJfg4TpCCgtKTp%2BnZPH0sqa1zS2HTR%2FYmPbVEqvBR5QqG8Gzue%2FoIFLCWx4Atukm%2FapU4dMhzDmva37jLcYIoYlrqLbAyy6JcnN24LFqZomGoP1IdNSb%2Fg1ELWVq3NFjmf0sGLcpeNcB50L7wiUCNwdRqAK%2F%2BLxQERPvNftF1yrQRjiZYjQv0V6ckNVs1oZMbtIdgO3HDO%2FTysuTsr%2Bu%2FZ2U3ou83bdYag8FbU3FNuqVbe4eqPGsobT3CidoodvRKp204JRk9hR3D4O68kmoD9AWblWfr7TJCS1Y1Mp7bRmcgb9XnYhnl1T45hOeLDDvzuTIBjqkAX32iVyxmhH5nL89p5Z6lUqI%2FbdkJ5o69Ya5VC2kNwlQbb11U8aX7WOWA5U9s5LhSq56wAzClUjRl2r9QlFEv9XG8N03qxwUXPm%2F4%2FEehiA6mdhLhDL2EKsMthfdESnNEGb8S%2BAK%2FTJcuTgA7%2FzmFML04R7SVCHo9qPaAwaWo4WI0BFQUXdUqeZNbvBw2W%2BsGexsjE4INzjcz7De6HmOMUtvOTRd&X-Amz-Signature=2ea38842e5c739676cf440bc7b311826f1a4b61c09992c9aa7aeb892d9a0cf20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


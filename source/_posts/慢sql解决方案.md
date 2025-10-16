---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ5XBW6N%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T230126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGHnfpSK%2F7gLX1futJUkcA7DL%2BRW2cGFi0wx1Cq1tYqWAiBrJqG%2BeQXjwIUhX8S8QZ58Qrz8PSdEUHT9IutYOVuVfSqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIo2UCPKHrxTR3or4KtwDl7jkvMDrJL05Qdk9VfoHeMd8bnVFREyyPW%2Fj5EWAP%2FpaxZPv5aIL4QWb%2Bo%2Fn%2BmLBAIM15u4fUaSsq1k%2BW6paTeBWVSPr9Evly%2BXOSN0vBW6pKpN053DaGLbS7q5vq0QNfHueo8pfjRDQ1yAr3ZyTr%2Ba2jGux5jev6V1QRbf2QSFZmu1xaptBgf5fZ7YY6vA53%2FSAZ2lbyc%2Fz%2BGpomGVStv%2F0uxgCcO19S0UUHHMyjdEcMHIlhwxHKGl1TT4SIInjgdfs%2BGKgECVzoJNqtcB0nnJlamU%2Bpg14iRVUWi0ZBbTLUGDH6DOY%2BLFdVcyBKpmSQxZL5UacE9MxmOM95VuEutEvkirxd8kunheLdawDmJ9SSJJ8htH59z5n1wv4XvOGXT2380Dz9dT5jgzQRajW50wSNve8%2BnAghN2TtJCuVTlFXyJ79bDK8UQEr0Pvc3rWrmhnIbE2wnyxOlYeLjTExkp%2F2Ww9nRyOq1Jquxqc2fFKq1orT4SM9HGNUQ5WHbF0P4DqBPRRtVx6EDtl7YSpUXRAdKiYv%2F633aiXsAjoSj0S5QnwCYq4IizP0RjQidDHaRUE7AQz9lGnuGIzoeU0nEgiuxzGmRfCXHn7Lbl%2F5NbUCsOBVtPHH%2BOk87YwmNjFxwY6pgHk8uVATZtBeB0WY16TMQJk%2BZxG5dEGh3j2bDwk2hJXPcoR6AWjRfGsSxWrAP6BnqvSu6RupUlayal1OZf%2B1uXBBBZMmVtzs7%2FZ2ngktA9IRv19LdZEKwtPEEy7vjL5jpyANAX7ok%2F74ZjDcIlWSyxwOZgFFkbeJEZ2ktff6Jzx89AS5y4s4VOwsH1vBOfq0%2BgdzloZqVzuJ2P5qxjNsTc1%2BO44srHK&X-Amz-Signature=72895b6b55c7226c20df53518ea1791a518acfffd68f1a5286283f99f5980e0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


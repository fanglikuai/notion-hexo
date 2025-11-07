---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSDYL5YU%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3jWRZKpTq3lt1bmluElpUN02wIdnsvjdrR2AAibT0tAiEA3VPMeZ8nVeq%2BGMZnWW88PwLJGdN2stRfiyugIa1BSEwqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDDTJXQ7kc6jpbx%2FyyrcA0jdq7cpu1WCas1c6bx%2B%2FYqSqGQxEHPRHIGsuj3hEhoIrJ%2BIUmWqT87Ab3oYrKfDdIyscYDCP2yIacbXvzVgmnVx7HNGD8YP2bHHDzNLu%2B3cIxHcRigDO29wjUOrjH88tGfDq6Mg4ywedAzM8nBkqgHaquHJJNEjhoU6ATp9nVvhjHSghg2EKfNieA56VGPmcJ%2B0KJOh4Lksypohb2Y%2FK%2Fk%2FQDdlNKzFeUrob6jpwOiGDL%2B9v5nd%2FPCpSas0%2FZGp%2FWDEbQHthiUHsTgHitfK6YFowtRKI7IzfXeN5u2Bs4TXTlrLVc3nT13HtwRREuuo7ooPHJM4rkNujK3UF%2BUygwF8MUVxCp0UYanuLCqXTNeEn0Ws%2FlyVuTEe0W3G3qmjBTZHkUOvT1JAu95N6ByOxSvsT4WfaI22Syc%2FyNf8Ruw46kj%2BnApzcDunYeV680QChtKuOL2B73EINBJTDVi3DgexkeHHlVOqa0VKKxK1cpBV1MJo5IfNtk0T%2F4T1c6XCFJ6nrTODsjKlTKbOqn9xk2yS8xGI4EpjjAzFtxljYvPlXnO1u7yvBgOAQprtb9o%2B%2FET%2FScpr6jZ%2FVorRx3ufq0gN6x3JgMNzcGMhTD1rKh9jjlocO3U4KQyQMIQiMKTdtcgGOqUBqnbKPzTIUNtz0Fo56aWpUVAZkBlfsdKHKvo3iA%2BC1bOjzWfRx2Xx5fBthLc2ZSwjLBNN6eRb13zZbhA7ho0ee%2FeyUPcVe860fu4MPtSxEt%2Fq6IYN5zaeFzNNFv87SVaZYcPvwhzyBBqxjTOcFRUFb%2BCclznbYFb%2BNuI27TWHoNWr5JuMPr%2FQOBzVVMSf2NP6l%2BHp5O77uGuGnfhVSx1xJFZqPdMt&X-Amz-Signature=b1ad1d507d1f12cd1f178834d36ff8c5702a8e4fd6276cb0adb9d3a3004f0bb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


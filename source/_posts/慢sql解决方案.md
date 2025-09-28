---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVPLTD6K%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQC2ASy%2F%2BWIxgsBF3xIiXKL2aduNQeWkB3pdXdESKka4ngIhAJM7BdhfJhqX5WsD90Z%2BGS2B5fJ%2Fe6me0WpLFr6aQa4CKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwa9Yck0piaLHHpXPIq3AMWISM4OgOvQ%2B%2BozwmPKjbUAMKB2tUHjD%2BVqHwU4XZzoz3NWNt0oBZjB8iH6yudFzUyLpZS%2BlLNC3mMUlVa9NMz5b7e2yCbDzFaAoFgPPERlHxJfjHUDH6%2BhusNGHJbUaDEPvQ%2FlQN1a1xK%2Fn%2Fid8COgP5CFcBNTSp2zGEXboCSYMi%2FxpE1ySyNu81LDykThm%2FBm2pCcwMTQ4sXn1jvoxJ8Accsqg2s0mcDK5lXYJWeqstUDZr8w%2BgQmDtBysQrjAf%2BcPV2G9MRieuamq30QVz2Vy0Crvs%2BX9AlgXqp5PRH1uL3sF5lkjSZpaftDJqDrYBI9kMwCxUCvdRVTl0D1hPTfClqzxYjH6Q7sV5Xfsvksz81Uq0VEQoG0H1anCHKGx6lriwPUOirXp4gMAgJlVB8DkyJ%2Fe4WGLB0JMNpAOzecKqnAvJrrdE8sQUWJTIP4zQsbQee2YwVOkO8MD%2FuzEVMhKzY9iGLy3Z3fdZkeB824jK7VApBXunzqcK4D35HWd2pfXX7I%2FHEG9OjM3M4FW4twT2LmHqCLjAsIsVxCDNX7cFGRJAbNu7U1wntsI5oMH6u317oIsyTXsymvvdcpiL5DdndwgDXd2H0OSwdUa81sV4%2Bm2%2BPCEUxGILTTzDwqeHGBjqkAYt4IbyHPiYmbRegYjiTgJqPrKWmJGa05K%2Bj5n0%2BYUJxEguFp0qQmC%2FT9B%2B8JQQaoRyQLUgpnm5Wpl%2BQxNI6aMihlCB7NXduA%2Fb8nqdMAd9YCcZj8Te1oafRRwCFUmaHQS5NeLrHSN8TpXBzxIkWGP93ZyXFD%2FwpgG9q8P%2BYDTPhaxNIFIBfhmnQ6JD8QJSXQNgAptuYya3JpjeFhwHg%2BkonaT2v&X-Amz-Signature=58c3b54921740b86189faad90017f0d20408d3c0ffae2d9d3f8a33cecd376ea2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REKTUSYA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCfSw%2FHwShz8U%2BQLgPPNKzNAdlnBn6AEzl2gI7kEqT0fQIhAIs3jBu%2F0k%2FbxPCaZri0o2KkLKDFScedEbjYZeIY6OvHKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwVkJvrXxNVUoR4okUq3AO0KMTXLFisZIVTFMx1xOh73%2B2QwjZssR%2F%2FtuNgstvDU%2B8wd%2Bbir%2BHQLBNP9q%2B9oYvw8dqQwog0fpRHvkZtmAtIt3fejJmN7hTa0nOdXqCi3k%2Bv0xxY%2Bt3nfHwiD4g5gmP32KB5BdQQ5ZYovZ%2Fi1xwANeIiRWgESNbkEa1QKJEuhew53%2BfAKRVg%2FLx7g2AZ0HtROQtXsTPrrukZsi4tbufgrZXCXN19e7bsorqf%2FF3Ac0ryMuxbV5a6fd1t3isTWQwJsBFMSLe6vEvopQ46jJ%2FzTgiL0Xm2s9QqtOG2Mvk7f1Kq8JGbcbdSqTTlnv9gQrczNsTdU4U5xx3lgF%2Bt05evBFHO%2Fkjm8QcqiVBxTjWPAvCetiONgFZF0g5oqs2Rz22a56SupnVziyCf1fptd25Jol%2FPaz93Mq4m6z7PxDAPyA4OuZ82%2FKhLJ%2FrvNByRwT%2FdkzxOmpEI9okJMnNXAU1toU5MFlGNfpyo1Fdrl8dUxFcN%2FP8T%2Fud%2B8edqYl2w1BEzAnBrfVL9H3qU6JAXQw6ifxyDPrUHNY14R%2BDHjHyq3OpIJaUfQvNYOX8a2R2ffpkK90KwoU28jEJSSiIp0LwPayHc7wNdSKQ2Af3GVPD9PJOy70bt9ABETIBy6zDX7r%2FIBjqkAQTt3Nlfu2XvLMhFdvmeFpDvEQEDZoMeveFgWfM0CWA2aYAaDDWJsTgwe7fSMsvcWjGyMMDMSVeYURKQV6Wzz%2BRiRPQrSIh7UilKxYSCzgRHWU71MpOW3iCUym8Hx4HldlDh12igWCFkkiFzSVIKMKKxfXTeeXDg0KrOvocWXBAxOcPUmBLJriEL94GYRexmnfMmehy27U30E2WTSFGo86SA4vjV&X-Amz-Signature=25564b51678432e3d7d99802b223b42ee67b410e5a1745b43158ca6b6e48802f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


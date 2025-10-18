---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDY4RAPL%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQDMDLtx82ecpGpxyKZv5H2kyG2J%2BhhT%2FpmhcfxC5CABJwIhAN4s%2F3BSlGSJkYywIhlCqsOq5TBPM6fSeLmeiNAEpk4UKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4%2FjzeB%2Bhxw9HHhxwq3AOa7eHl7Y%2Be34%2BuLYZmzWrE601R%2BDzfkjOwwbg%2FkeIb%2BKbQKjwBfcopZKo655B8T9BzDxuXxyICgAxopv0BcWnpreCORcpYeb7Oi%2FXNQ3iBxWQ%2B9ffIDVmxhML8rLUY41KlxfGunE74bqXno8DRDpsiWlktx5TNR4emq3Q4BexUrSW1J2kOogkXSyOo08bBiVBOnp4ZmEl%2FLOqpau8VNPwKCZyGyyqLZurFrE1WFDwwxeED4Osc7vt4oDB%2FVlCiBXAokbDcPGMj185cr%2BuEjrsm%2BvRQKspFTC48iopg%2FygIdWQMzhjSJx7Mqf7lxx50xWWziKd5fUKw4dJgYsFMQVErZBBYRAanEyF4iarMpHvAAftvgAb94QOHULlmr2tHCnlyKe6IWhCuzBB6aANEd3tiXRIv7MzcUrmwEj8w0z4UckHZr3JlzsahmZHLgZnsIRTMllvf48MCJkoaIDyndi8WS2XRGtga7QAcidxkNkVzZbcmhrNSzNPlcf9Gvy3%2BTCQG0dE%2F7YIalICK9BTx8rnbEDlOdiCDEoxIfPldfMmcITqP3kfdDY1%2Bay2tAmGnQ6tQwXHFUXRZ37sz51To%2Bv476QeS2kQpH3xoXFjbA2pXbqBWcCm7J%2Bb5s9ANdDCZyM%2FHBjqkASwXJtH6xllqmtuMCEGoVoWt%2FieHkD6qlIzcNM3nUL%2FMe58CA%2Fli1spxT79YSPoATm2qDRtbEv6sS6JYWJJWTrNUWEiBRW9t4kyoY48R9bFDHyE7UFQcfrmzPjBi93t%2B5%2BZxAf4p17NRPRjEElyH7xIkAIUMAzqKZzAtweE734pCTbeBtp%2BftcHFj%2FoejSDGOaQMNVV3AhX9v%2BF7GTgwJFhvY3jK&X-Amz-Signature=e59d827633dda59016d93427adf6b02596dce9789779400e05e412cf8799cd66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


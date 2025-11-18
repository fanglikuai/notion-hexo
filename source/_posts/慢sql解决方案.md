---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664I45ORPH%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhprBeyNA%2Bbv2319r%2FaPL6pR0ee8%2BnTx33dlP%2BNv2JpgIhAKqnwTKJ91MXVYpdksEfH8jgnZZqMzmU%2BTe5BIPCcZNcKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMk%2Bij14iOCcs9c8cq3AMmMZug4VRimcfRW4egtFz%2BjJx6a0mkYrKIcnmIG9to8vKpMBR22w2vEdxXHav9gcvP%2B1lDNpdnyIpokrVjh6z3IQeYh8weqElHE1k07%2Bbj1rX%2Fc72XpEqQ1lrcFTbjfUCD688qe0ppDN4s7ABh5dJRfTgW3Gp%2BpLygCMrrGbhf%2BJMUBlIodPcDmQLiN09Fiu9aqFWInv9jGlG%2B%2FuasqO3WjiI%2Bl5p8BjpMf5OQHzzHAoQam3BmUUvW9v8fpXGEJeYJOXiZbu%2FgLEiZ73Oaitatc1xjWd4rVB%2FiUlt%2B4tigb%2BjT8s6Bx6OZIL6t2AU5Q0%2FELDbCM0iiIh9%2Blx9e1WlbsupBOe79k%2FZvcKZ53XdRpNQ8UQLO7UcI6%2FfFMEk7CKgyu6O05j0EBm%2FKUADEzmOOJAkGozfFevcDvnK7m5ET6VErYfNE2GjXFL2swu4wceS%2FmB3S3My%2FcSqxzu%2FkN3DGTO0xjG3%2FlP13Aoo4g796iMauA0dVlwgCmqg11mBedANmGPZYaHcn6htkZD2sbqgsXNS%2B6G1coFP%2FRNyx66tffF1Z5N85LSYruvb7ZL5F0TM7o5i%2BfnxYemuVY2jejnNH1vZHGHCJ0tgMN7Z69%2FBHNFYJ4PRQ6FQ9rNIAwjCOxfHIBjqkAXk8IVqfh4hAOsGIRDsDT5AAnM5bFv3gkrqXyzOPwJ0X6AOP4hJ11J2eAlrSeakXNq68FKN%2FFWxy%2Bkn2EbeWp9Q%2FSTmFLJlz8ZfJzG7qH4FxBixoSJs%2FqSQQenMBIVCqT%2FmxOb7TDTqkOSt90sC9s3k0o00WLruNlMCe%2BupnRfg3e7v1a9SKiTq%2BJ3rBVf1L1CAOQIh14yNQ7MMxZdU4hfmRLoy2&X-Amz-Signature=ebe7e53e649cca072aafccb8313e4a1c6ac0fdb90ef0ebfe49b945cfdf3a58b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


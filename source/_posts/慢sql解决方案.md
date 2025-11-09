---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVUPOMMA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIEd97A%2BluO240HGX6lO6aBCSprSCWKE6iS%2BFT7S%2F60HEAiAI2SNTHts6kcx8b%2Fj5T24jVQ%2FgWHPUA8IKLHhdQIz%2FxCqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlFX%2BQOamDiUSQmGbKtwD82mF%2FJoPwEU%2Fgvo7IAl4fEyGrT3kn4mNWD1%2BMlgPe1gPht%2FrR8%2BkySap931dZGSu8IglM9zkx2dTsZDqRy2uA9%2FgiUX749Birmd5ZAz%2BM9XjfVAva8dp2Dez52MoUPhHSmIAA5JPv8puOCCFSba6DqjuW%2BCcnVRLayYEnxUf1JXqeUCxdf2r%2BaP%2BrG1me5MSPOxgf7GeJzKtS%2FUkObCgef8iO5Z6xpjL1DT%2BF3PdJk4XFXDLsD%2FqEScD7E9yd1QDfB5SEm%2BTVmQbQccUxwsZgI2tHdrQhzBb0zP8jVsb6Z6tfgeln6OjhJ1VNtCxbTGTsqisYR5doLzq9Lgx9yIa08v45Vd1gDnAqbSNnpTjkHRjEFUfbyV6h4YB7X5YfAsgeaW2%2FX5RKdD2Tg7InxfTOLAgP55rJVC4BvE6u%2BHwC%2BK0%2BlqAePHmrN0w8ZUcjgr7AOReWctvTl3d6R23sGfTpDCUMySFPKc6pommRz4ehW7hhtu3DYIQNDQj01aIwfIn8ko0P7KyfrFclurxjgTz9tM2JL%2F5QCqtNcrkXtH59fhTuIiS3GmqbcRkgVNIWdJ3sJ25FrcQNVV93FP4V0DYzn85x1x8k4Ss7dnmIcZvLK3H5Dlj3lm0wJsc%2FBEwmK7CyAY6pgHdseNyaQ4ghKgAI0CaYgvLxpzRwE9Qq%2FQxwhtBaZDF8PpsTZc42iThZ7GG%2FpNjE6%2BkvsHvXYSBaiWzi8DCPsvaFh5bfFZNFrgZbPcBZBoUBOBHNIYMwMKH9yHKPFH4I5TD3VucAQzGaSN2mWJi7TeaSsR%2Bu67FScuY9Vlt7fc20XELJ8zGV%2Bb4tphZDK17mpnNohDt5Nw2ds%2BitcdjeCk4jbGH7hwm&X-Amz-Signature=7e960b2f30faf888d2c13248982ce3b11b4be76fa1f26644a833a745771c2412&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


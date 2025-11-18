---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WG3XXE2S%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBGgLHWzYAICqDFX%2FewYprQg%2FjamiyVPiF82YlLlHGP2AiBEeS0YPfjFb0wwEo1Igm%2BAOTEjxcv0QlQMKXc6EgYxpSqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0h%2F3pSw8JwiBzdvLKtwDkFgXXm%2Bf%2FchblOn0TD%2FckxGdFkfz9ieNhu13LSs5JRI5iHsAzQNnUPfrs683ZzoDXe9q0eePwq6syTO8Ur%2BiZbRjTAvW%2FCM8CSD6uGk4ux06cBhow%2BzlRSaH6atiJgRG%2F59ZhlyBg3D%2B5uEqGIxxuxV63d6ZFJsaZ9CWaGmVnbs4NIJf%2B4PVH0pGi3uhTIV%2FSH2ZXqdlJtuNkmyK5tHLLw7jlHlsJFHAXpCgv1SNrBlXiPmRcVLhNekx6aSILeePYEGLlpMKk4ivPrR19OOndi%2FOLKBjdT1UqxBrhomqidfoiwYZm4ELTBnYZthv4FeqUVH3BfSUa2hOymEHRNSWTIQRyrgvjRJfR4qU8YJBCfqWzSsGDD5J3p88AOxz42PXSULq5tEVQPp4BS4CpiRGeavs0yg6WPQq9jt%2Fdh7jwVgQNohDJcpvYKk4DWA%2BTUBFxuihb7PuPUyCpixqSJMURAw2t2ppkoSZS7G0Uxlv2BFipB5UNcUWR9u5FxZrY9KLZ95tto0iUGCq53XEZUjdGUO9IxxZt9Jf1fSbflBcFFNesyHHkBzQlh1GU73Si0sDQStMYXGCdl5VL42OGvj7mFE21DdcdBKX5KO%2FbPjgKwsG9CrnMT9MwJzLokcw%2BsTxyAY6pgFyibE3vxdRjaUa6P%2B1%2FDajMnRBKAQbyuGKd8EVdhEAkpOHVeqh5bB3YCPPeH1fsttGpfhbflAKRgADqWGkyWS%2FLyQ1wx%2FeqXLeTVK%2FwsXlVCeUaOwqJMBxUEwTCamOt2oNBZg8b7PrzQWaX%2FY13%2BJ7rjGW6JFZqk4BT%2B7v8mV0PLjBokTR1MI1jpI4eZCUUc3suHijiHiv6uABwqc1LEoT6v4zjBTf&X-Amz-Signature=954041bdbf5534df0f6d5628e2e36a5fbe88ac865b2ed46ec19a337d6cedd620&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KICVL7F%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiQKmZYWDL4%2Bx055TPq%2F9ogxs9UJ9ZKbxnkesuHqAoNAIhAKBGHaLJ5e2npbIZm8votpiFa3w1a8piy3tRbfrYUay6KogECIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FfOHY5PSv22jNwO0q3APjFVDQevvNbs8TGKcTYk2JsOStIwJpr5vKFFTFGTo2rDv0wb%2BNuFKJCrtbGa4BL1flrz1al0DGj3fi3mcIWJxAp1YJG0KVP7GbSU7fYGfh343vNt8qh6kmOcUXRXsDR%2FaO4qvVdjzigAQirBylLM2zNJ7pcs5qPaTi7PPNys5rZ5wLVpyEqur9i%2FuBC7vFDRKM%2FJOKwd5Aiwf8YjxjAg5Kr24jbUPMWyt7Z5nn%2FFgjnSGnpS13HKxYJlwuD%2BWIEN%2BHaNsRNtSscdA4BRa8Ygk2Bg8Tlpq42hmRrP6U3IHq6qA39%2F3uCiifDpA6lU%2Fn%2FYWTtjJubl7fAywSBARUourUIrADzYd4mCCR8aWZ7Jdd%2FptkIhzpcOxGsyMx3fau%2BqNGUo%2FbT8FnIMPHeKrbA52bbWbA5D4U%2Fj8%2BbK6PUH9eKTLUHOqjhx92sffRstPSyZmBrQ8awmrc5qV43RvEMSAKYX34neQJ9e%2BDBS7im1YTs7XpW6KvJ%2FHJPAjE7HFiRRW03dKefnAdPXyfJqUKnv%2BUTBiSMt6%2FplrlPz%2F5Yniqm5PXHPtjgv3QbxUfyGJ92u2nFZ6Xwa638ApkA2avTq6smAMwIRAAP2tkmxzkAz5%2BcNZvO2s0z4shx9oJ6jCR%2BqrIBjqkAUzmo%2FFrT77Ax%2BM9pOSyJKInYbgAvnwbg8Vyg7lfFlY8%2BEGsrRNylBql6p5fJjvZFWsaJpI2%2FxJaMhkzQxq7Iv9203tyB9uB9cAunHD6qU0JjeijGbMdhzLzdNDzybOMxu8u8SOD%2B88zl8oeT6RO245znvgBapNMia74oOi8MpQgg8kbeIlz1uSGjnZ4MmjpLPKb4xDfn5qzSqwpqbYT3k8Wb3LK&X-Amz-Signature=05eb69bea71c6cb73895ade438a1fb2e341a30b5d722442b1b8cc918ad5e37ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


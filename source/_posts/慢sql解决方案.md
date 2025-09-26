---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UO66UESM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQDk7G3%2BUoufCCfDkEXvL13PCL0UvYskAAHiEO0dCiRCqgIhAIEpgj3no9m4oDVwxw8ROhyR3q9E%2FCCQWjLKDeiNsMSoKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwz%2BI1T7U%2FaRCJfWKAq3ANVRWXgw9dNKBPT1eyvVu28%2FhF4uJdPtRPCKOS0uncIkRvfXfBkJV3aEeAJpf%2FFtVU7IfC%2FM9eFbuFepFp7LWSLsqq7Pn5cJ4qi33AwivSRd%2F0TcXLJtv4WkNeEkFpSQJ5i%2FDVUdp%2BDi9BDTLA8vjb3KYM7CmT3Ocmy6SFE1LG3G4EgpmJ4mrFdXFLiI6sZcFHpw4062x3mh4UqwhZTlouAOYVQSh3bAnaM4dDBN3jt447lzhlJ1p4%2FqcGqZwc%2FxvQuoBvB7vSIIJv9Hu94JDQ%2FudHp3E1O9FejeUw2WlGacBTGAfMv%2F1hnI0K6V%2FGiEjK7N8ZN8CbNsXgxeDZkPEdCIc8cSv2K4%2BDeufVbDnv9dfSOLCi4%2FXBCreXnjLVhDBLgA86vU1kXaeamByLWGKAWH2HtjNAhYspRE8XH5GbFdev2e83JIB1Pv%2F%2BhOprJDyhRGpuKHcCHwDtf7LF%2Fi3syD6X6LblvocxEoeP0qThYj0j5Nw28%2FnJpafAGgkdinq5Won6WLQ5WyP6bT%2FyzqQMo%2FguN4c0PGMAhhaqJP3q4UFNKLHtP0wsVnWHSqNhNAE2fybHbeIcHzVFgZMFlsi9xcNqzB3pJY4C2UDmmeNZuIs0vfY%2B9Tu85kcm6szDC%2BdnGBjqkAX0jVr0C2wIPl7%2FYgELKFp6GlFr9vJyPyD5g9geNG4rSgJ6g8ql9VIJvYT2g0Dy1ElPKb9zKI9iXmWtu66Q0Gymhe3ZavQ8qCo60oKZAjCoA0Ei29IDD8lBg4v0ZfyxpTO5%2BhEsm3T3bg98HoJt94D5C24VeXgf7ezsgTZhkHi%2Fe%2FSOqKknwgL5mudK4FU4vTNusC1ST1xGW13uloBqgPatDVYkJ&X-Amz-Signature=b19c227a04d9df45d9df7b04c291df977f01c6cd41c2663bb9c1a11a80c828a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


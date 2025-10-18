---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBAQH72I%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdmZfCbi7yuOxHM9smp8tT6pICJ7D4vLJt%2BfFNWqT9dgIhALEP6bSWhWSWaRaM4YyqjU3oYkFU%2BdXONnW5vZkvwwScKogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaYQo3OGWfg973HEcq3AMxX4P12PpefoATQsfqfzVni7xjC2c9B3MZmUD%2BCtqPvCCrESw5w%2Bk95wikbL2hID0qkyWhUyFQi2q8FNhb%2F7CvjaiDLnrFGJAeculHIqB68Nrf6FxY4HxWR3VQ00Q3U5rwPahJQBH2OcvUK2ZiPAvX0wOkFc1GhiHCmdYWqigWtG0l%2B11BAh4J71uMImsrKP04LSQaso9WBE6POAmkBuPmeCPdijn9DxV98WKg1bkdHPVP2ya72VMgeyaSwTc1oGrFCSaaLb5T3j%2FpZR%2BQ9gUVmmrKOji2VQyBlS%2Bf%2F0DNV6WgHFZZaCrr8A2SKpmfi5DySbrmfy%2FSd%2FjTU3pZbwJ%2BZyeKwguyToJmtzkVa06izTGZJQdGvB13xfRYUnkO1amIQYbAIE%2BLfNavwscQtxSY0eVDpdnLSzwFnK4sB93QV0AhD4m0OhrqRkakAj7jsH5qJNKAkw4qWoQQSQ6FpQtpfqysYoovzyjeL3Og75xHFBkmtAvL3jVzN4goJLXAweSk2g0JJw%2BUEnkKRWq%2BUiL9A6AM%2Fujq0j3xK9Tkm0MXbCK%2FTntQL7PxFo8YQvYC5NS7F3%2BLZEPG%2Fk0m7yD6cm9WvrWmQNBbuM%2BRu4CLLYPDxRUAOtap2bWRkDEuHDD85MzHBjqkAV1djxi769IjT%2FA583bVWU2bVxlpGx%2FS7h0q5G0ogNM6WwrIvnSplym6J7vz%2Bmh6DyBm%2BZAAL6zasXpUoPSHGyFGVWXfapAtqDG2XAe5vwul1getybbDcLjePIfcZ%2Fp9YHR6uheo5qrvf22V8FUpTXWJzNkJvVL6%2FsIXD9T5%2FIRUYbr8us24GKh9a6R3acajMAlAaAeGN2t%2BcX99CQK9sTgcFqa0&X-Amz-Signature=75d5c54f9c733cd7de414f5ba77442d6a866d4164450ca0df233a6d15ab719c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


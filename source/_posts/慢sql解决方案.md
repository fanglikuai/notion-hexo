---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZLCGJN%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQCCYkNXttw5RWHJ3OBr4TbpGxD8GO5PGsNmswbIdgrQpgIgRmx%2Fr7HMZpzZgGCYZdKPdL0UpwXSopiMCW0gQVIj7xoqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKrKDgCAVMzNGI5DVCrcAwcANL9myXWY2e%2BU7h18SQCrmGVfdgbDFq6LVFE9EINnEb%2B2RT0AAKfrrsPW6X6YkCbqp%2F4MOR9dpoO2%2BTBL%2FmodeK%2BRegMnPu6UWY%2FExAKo0p7HLbVZGUnMNeJ5WrUvrQ1WzmPj4%2F%2BLhpy9yRxAS01S0oNOqou%2B3WAFon3KD4LE6Dzu%2FuupP1ZivbXobFdOgVzAja3YCg1%2F25WO5GmUyEqHE9n67tSaMrXckVNBID91Gyo31mhyDGZi2DPV%2FzCNut2SAoFU4PM07GdbW%2Bo0mh5lhb3YAysMPzcgx%2BJ6ofR9R7lcLwCymMn89Wx3LO26nMDWk5D4EwnV%2BElz5RDjBZe%2FuFGbwNPrGF6r7m22l48gMydyaird2RvGYlk3yPxhbF7SXrYlrC48LuPRH8fmxAnJSHW9KSxAY34ewe0T4WIRVbBWdjWZnnV7jsLmjZFHQahtIVZpcKbiMdEq6uGou%2FF29UXIKwND4KrTBlzloxLyeqYaOjdX%2Fg0VLY6FpWsCcKTTuJXRaD1%2Fq4vXlC7l%2BUyJgIycyfRQrJHmrVVbWs7N1A1dSs2m9MFCoBwT6ZWwHsegjVgsC9D5izDGCCJuouAVuq2bE6vOdxo8fR4ujFYoRQgPPD2J%2FUIFOxd1MO3E7cYGOqUB3rykU%2Br8k%2FkXDRGWj1DRxoHGYeRRD%2FohfR%2FArdbwKsrgi%2BA8ToyP3o5H17xmBMZye9cpynvX3vE%2FEMcTWs5fcPOgtZmgtS2dttYA%2BqCZdNG8dswcjDpT3HlizvCEaxUV42NgwdWQCGWB%2Ff0RPOxmcuCz3ZFCtv7cS15ft%2FmiTVnVEy6IngP6bL9IrE37aFbaMv1GhrZG2lvEQB0TNaPSqYch4Ze%2F&X-Amz-Signature=21f53f3fcbee7744c902c627c5f9bfe29bcc191334f1df2c3434caa6c31d7af4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


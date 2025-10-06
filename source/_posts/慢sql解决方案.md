---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HK4RVW7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDD%2BPtDuwHITYb%2B4%2BjaWFEnAgEcLssSm%2FfHm84H7jzvGgIhAO6pGVUqiPXK0Z6KfcHJzQpZZrTFGLgHUHUPTEsH2jPLKogECIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgYeOUY8unepIgD%2FYq3AP5Z%2B0ClROo909v%2FkOGvMVJPb7wZbKqPM5x0ek3fjJyleVQjPkjNI%2FSiDSouFutXYqGS0hpQvPBeiwyiVYKYo0RkGX3gh9GpWr%2FO74Dws2kd6NmDFenVp9Mt%2FGif1hdHeNqEHmGPLsUAHSO5oPZFEF87YjBFC09o%2BdzUJmuDjYOWEFC6zPWAnaYWkPA8Se1inleBtLMTM7%2F8MExA86DgfSIVVMJdWdLAfP%2F889%2FQrcW740i3POCXSuk0SskH3fC%2BykSbQkFK3p53Mk5y7g9jIcl6EcZfgv9WsnCsP7CGvVL4bqZvwc9FcGnecYJX1EEw8Rm7Oov5qnmwzopjSbThEiBqkwbUBdBqOII8WgiDR15wdxKWAUcitnGtsO6VVfgbLZLeWeQWGMEkC5%2FPL9fMEz4mUie4ABMgEnlE8hemsJfa125obMxUBE0h6gMnW7PDnxIzUALRfFlWX44%2F79RwjnA6whMsrVWjVYuWYZeIP3iB0vCiCEw8Tv%2FKBiwnF9tadZYwBYxFU5%2FC5Zcj5v%2BKOQuBSKGeMXj6DEGk1vRI%2BDk7MQpdT7xJoJkOEUvB3463urGt%2FpNRm6MbzhAikXWOvJSdykewghyb0%2FatGMLHoy1yNCdvlql%2BVXrs8X0cDCQ743HBjqkAR0URR7pwfl9OCMhgES%2Bj%2FqDDw9W%2BhTpjJ1ZmgAAJ3xf1uFNALXG02J4SaFmIyklOFdOSfqgKAcA7sgqYb8WWAUO5MrZ4wFH3y%2FKVw0gaDPuOrDR1c6acsPYwB%2FykvgWSJGOFRQ9pIaRy%2FzRA6XaItGQ%2Fn4%2B9k82VzTWOy65NXHJ68Nzb2Caa7uvIDrHJAR9mtxJatxcmN0NpLNDExInjIsmvubW&X-Amz-Signature=ce7cf0798e902e3df91949996a066957c53b42d8554adb8f3ee99f942b2fc509&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN27GLPA%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBmiX%2FJvtU5p1AmilLDZW4P93B8XtkOX2tLdHNwzwf21AiEA968XfSpNq6ohe%2FDtq%2FaIFhA2CazWa%2FF9361%2Fo1WTs20qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMaqT4vqxAwFLeuwAyrcA9Kcddjz4vFGg4BUrmdrzyR28Af4WWpf%2BFwKP5sRpLDXyBKM43qp6FwKIE4OjiMMY0jusxJYh1j%2B4bDR33qo9n0nxG0%2F2hF3ICYsF5TtN0hDENg71B16M42J6C5pSMRuVJ8P%2F7MQ%2Flz0U6nW2BwKhdQBG8l%2BIq1NNkLMaq0wdfftw%2FzcEkyT0EL5ZWIt0htFTTG7EJjH5JJmfvdarxYzLtkQlo9Pw0kiKHiTAaNB89mHzlLves7ZARuZbzfRPo1cy5hoA3kivPLXd%2BX9vSCJSpmahXOwYrvqpNvXPmIDW4YewWtSrm%2FasHMd8yTL9xTFJ0z01Oh68EGgcKvlrOZgBqwZ7MOhIQcRxTq%2BqZDbj1bqvcTUVb0xvdBGA3mT8jie%2BqzRm9uIYaAZ0emQyIG9wNmPa5nMJY7LyjAwkzV2TQQWrk%2Bq6KFYAmVMSKXoejamJU68zNfDSS7AMKk4hrgVcaDnvTV7bT5kfXnYNAECubHNbF6jJctKijUFiNvORpH0g40wbnCH1JpZaFdeo7EZJF56KzJdBZnaFQBGNYgvYh1E0puZrhTxnlFwLQwr7hYr6K4SpIRflMxVcVRQby9nIlIyERc0TW7rihX4UGOj9Kklk2eymBMJ1AHWMZkzMLnmx8cGOqUBzuaahDlMWIUuzoPAXEj1u0xaqqg03O5teN0FqZoV67ahYf8DQSueeacZpZgCacj3XmTJfCIBFl7SCfZ8Gh%2BNeOtGS1j1JDbvyuPFMBHi%2BRw9zqFiTOKcg9Vm9UfANMwnguF3L%2FzB8OySKBSqFBYKkR9%2B0Q7wVyML9hQDEt3A%2BT4QGqdyPBi1BBDTIoFGBJeKpHQO3sIlmIrgr4LBD3poaQJJcdAa&X-Amz-Signature=2b45eeed10af6a9bda4db103cf0aa555faad24ef0922e85c5975b5311e92ae4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


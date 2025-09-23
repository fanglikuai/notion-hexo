---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VOMX4J4R%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbRvNpjt46zA7sZo56a47CQkA2qi2vOXdiaO38hr%2BXIgIgZtiSLhB8p99oqNqj%2Bnx%2Fz5oUAG54Cw%2BHvRij00RdT4Qq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDJqLEBt9lgy8LnzIBSrcA%2FmcjTNIyyKlQhrFOSvCKqlc0%2F%2FkjJQUQ%2FPHUl9%2F3RKcI58YS5NzzFJ%2BOMuCaaR35r7NLJjT3ZDi949HTCy%2FuuBpDf%2BnvguxyipR7B8ycAChcO2JsKRg8A0fTorXgwx%2B%2BWgKYSzbcAvGmZEtacUrmJd7%2BKExS3VerPDu0ZYZraWcNNmjCO5%2BUz4cclVPcf0GE2SUpc92EZsWcYi2g0zX%2FD3W8JJVBvegFiMQEJHa0lMOAVE6XadPMJSW6IXX10QAVj8UtIpDtqpUKeuKR3sD2ryhj5MrLSvrTcIHc%2FwK2L%2BwCxBatmFf16gUFQA5bWmUsRZZ7448s3UQeoJp7fM3BpfwcwBzIARXum3hbRd8OsBmn7%2B4UhzHuVN7DMM%2BdV0MJ3GbY1ATz6P7%2Fg8oGd90yexal%2BHf5OAKsEFQ91y1laMA9Tb5DYeeS3WrcWRvKMdVEwAOPI6jx%2BGJDqYaAtskwRh2bB0qAN4pJunlBmPebI1wcE2nqpvjbdPZkJE392DX2grkfB8al3GmJB42LP8TA9CTAEh4fSc0vKvqRBl4EWD8JQG02ru3tLZ1eeLCbAMCNpX7Z9q%2BIYOer%2BskoEvqyoHztiJyYyA6muQkdPHFPQcQgdY2ezAzBfLHGlQyMNm5zMYGOqUBSglh7JV1%2BRlwIIgf3hABloVrBJGsDY8nWNDXfZgXdATFktzL7bFMQn9Ch4bNOiub%2BbTNiV%2Bagf1GfW1zTWrx8ziFz0Pa8BjdxmLO%2Fo0awrfNQzyPQXdvdEyI0n9DxXAZAfhGLJd8HS1Gtu01%2BlmXHTCDCzjqMoa4pH4UdxKZkJXjjytlwUfr2N341cA5Svv5vwp9N61jS3sFgYSIBViQmt5eEDKo&X-Amz-Signature=6fceedb85eda089107ec397b685a0c16998e0bbd1f6eff944f00d9ef5c3c53f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


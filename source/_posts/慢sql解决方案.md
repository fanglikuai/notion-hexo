---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356YCOEN%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T070041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRv3p9xS9vK8rhsc%2FHF0wIypaot5b2qNJ%2FEKIgCIxL2gIhALC4ZFu3RGEuCZGEc9M%2BznxrqjIaYwySrxOx%2BHtP3e5JKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRl1bUOjWydTJSAtMq3ANo4xlhZXO4iSlde84fHM9%2B5ZeUk1uESB3YukFcPYpJMgmLmpK2GCNTRSApK3zGp%2FD322va41MfrcrLY9vXWr9QvR3SnYQz1dZKHKe272ZAFd9F%2FDAfnBARhc3K08qQXygZ1AzlzUQ7L%2FjFMP564XILb9FsLooFpboWtbCrWcG5j4JpVweFNgi8nrvHvjMESNFk4GdHNZsnrZxNGug9fbvgrF4kX0%2BDGlsHTnc0Lecfe2aTr6lpNNJKxPcLfwb9ke5DgH8Mkiw6%2B%2B6KG92%2FGHsOwUKxvZsSGneql61j6nmxUEXzxdQO86v%2BUC3R7XlGcjlD%2BMs5h6%2BZickzD5trGyJxSHHwiDrrhrbGg%2FsnYLtpRzd5q36%2Fq1DkaNIyCvAV9MatHV6H6rbIFPpQHCvQcG7WTId5dQcJMj3kOYgYfgyuw70AtJdJax4WZbnPIVMdpWKQFBduSRVLkUzKknu3Fd5mCMAj8exd0LRZmoue%2BLCxD%2BsqyAHdnIGeqmHx1LIfTvqQnSXHADYDpBuAzORgJIZVVT6T02N7Zd8f1PDTe8cjLRb8loY1M0T04gfgrIG2vQb0%2Fdh5eeITH2uAg9IZxv4OLecVI8CjFl6FuhVnsz28%2B2JkjOoLRqt0t6KmsjDB2djGBjqkAT0zISnIJofSztL0W3qsH1PpUHMpRVqs8KC8cSTLJjxxpnN1oFUVqL2HDtWKcaZXZDOdA%2Fc8UJyPkPbvP0cviTP6yC1Stoftxeq1H%2B9JhPJdhJCB2v%2FdC5PlM8k5QpPDR0Kg%2FKHI7cV%2FD3x6HTh4lw5KP1TZorDpwgHNvyv0PQui5FH%2BT99KJwThVHgq817ZZW21gg%2BdB%2FNLb8Ho7Hl926tMjK65&X-Amz-Signature=dffbda7a21f29b97b5f924704619586e6e826a5b3b6f5b1a604e3c0268fe48e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


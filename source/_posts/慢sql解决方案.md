---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTT66ZS6%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJIMEYCIQC3%2Bzc61VGGey1aLSyP62uU2uJZ1h2iLjLFQLLd21F85wIhAIxGoPMJHc6chQuSyZpby5z5mkrYL5p9mjZfH5jD8oefKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzhcUc7W33TpSfSXJEq3AMpPNj33%2FcrAd0qmTczRK1yKC6Y5C1OJsxnqNrRxDVyYPuI09f%2B4dkxtP0qq3KfVvhJQUgFYIFoZQKB1tTbQ7%2BgOpoOW3hFZk5QYwMJqITrIioTZhGNKwDT6PHDPmWWi6NaGR58PyJ%2BUNAcZj2ArEmp2Y80viRHa1ymTgZIThWCwG8JLHazuOA5I6%2F7%2BBznuGvorfAwA5dOqWCuN6lVA3schfPlP6Am8y%2Bcca2kgmblzcq4CDp%2FZhVvmYLU7dJbPsjKWUfDvHKfWMZDcL44N5jhVw4cpNzfl9Mjw3nBPrmNJZytBhu4EJfs08fMvYCC%2BS84Yq1ILkqVQzPpTlfJLTL%2FTdGRbcPj2P6tVvFj0RpgVVQk1eGvhZ9zOKMgcT2gc6dgfSN6A0C1lW7HgUdXT%2FvGIXl%2Bp48RSKacKMj7j0rtLBUPnsPoYXUVP%2BwsTdhAGBiTOD%2F5OfDT5G40g9yyNLTEUklEfHkF3snbB7WC6mvnJpM93NRm8SAU%2FvA5LrjYk8vvPs0drtLsI63e%2FwCiG2MG%2BQqViRe4jjHWiHLCLC%2B%2B8fzlWKqzkZGKFoLBsK%2FQxUKhLv9p8CeXGtI6UYIl5KmQGW0tTXs4q8pM7JTDRh35DwUW0NvM5edw2efGLDDclNzGBjqkAZyptRPqsC3jiAo4Bd%2BmnrIeU1W%2B2VqiyzvO3kpvCrruO%2F1HOXceiOKrZI7P8sr8EKcsR9jXIpLvS6%2FsN%2BXDiryvM5G%2BnuKqHM%2Bl1cQxBDlqGIwF0m3FtRqBylZ%2BWKLJMLi3J%2Fh6REc0lLNMr6Aq6o4AEBmWI48mPlPDXvtg6HDISTsBNcjzXJRVPSOPMCTnnUtH8pNdIUwPnI8mBIosxXMZM5z2&X-Amz-Signature=12bd2c4e1fdd5724922c9e32f0aca2ff6211351d9e2940f13b8d0d5958f7fba3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


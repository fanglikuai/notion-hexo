---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWL7A6WX%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5Ii2LKjYG%2F6nL8ThOHdc6kMS0I0tdmFudZYzIFAHRmgIhALWLe557%2BL1zFRuhIucEHusZuY29FPfh2juVTWTmxGToKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx7c5M2Br%2BeFQJWXfwq3AP5Fc%2B50MugBjEaZIAFFZFIrClAqaAhAmzBw4X6aFxqz3J%2BJoLMfAGPjvTb5Wp%2BMFxipqTcqyudENlAoa4XKiPQCos1X4nCw3FGwShpswtNBsyrJ%2BwWACfC7n46VHTTQSahe27zTGlyZn5uSOxuRDPrUdMApws0bC6hgwjd3kn1tvHcR%2BG%2BROYKa52tsorDvwU2eCko03YWBHRlGIBelS9DD3E09uNxwWbmGmpsPDaalD2vVy2Mdby58LoPEmzmd6LLpy7l5ReCCXQCq22LgpZHrwLlSDvr2OIM%2FrzJbvoSqm8LN1T8efun961Fz%2FQNREbykEysN0aCiE0yyrKZKRcnQZXWZiIIHYXbN2X75%2BtsDrzUk1MVfQnXdMYuDSAvBJK9IyS8j4Qbma5hz%2BcGAtmtajIX%2BvzYgFz%2BBVR1gVVHr22zlfhzTu6V3C424Tp6eJGRbecpvxN3AcQWhqf74I5SdtOaByzX6RUiQBsMkQubHTS0LBHeiIoY%2BzPLgmTyijbPxhqnkhnLxPFYJ%2BsnQNn6YLZoJaN7CdBLkMUmV2hNk1zYtAhp3p6hjN3qFFAFbdX3YLMfB6WfYH4Ek%2FdZ3brv303ssch56Wfb4o%2B3y2JhVU7tI1QVoC%2BDYI5W2TDe4KPJBjqkAdo0WOVca0pl6noyobC5E1SazZNKyMAjgABWdbIR8OYZp4QGlF%2BQ5Mh3TgfZrIV5lUi1nliwRiLnkBhcjLTT3PLpzx2iYlg3Ytglwr%2BNtDUk9yJPO3Fq%2FD9Eaq7KQT5GufXmORwmr9nCPdcIaJKO48xwwXC2IjoYVS1DTyDEMMqWtvZFRzIVaTaFwQgJczmgFqaJOltDQ2%2FmpzG6CiKhhlCXm613&X-Amz-Signature=8268e872cc27ab08da1050566197ca45793eb4ba3e0fa3ae5b5a09b0497ce127&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


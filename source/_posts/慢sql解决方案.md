---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FDT2UPL%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDnAhyIks9GaygHFzoUOdMStwq3xDLC%2BPm6jCtOdRGR5QIgEXpoSrg211cf07oIG0YBOR6wnis9Fe%2BwuFgKlFvNk5gqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEx7pgX3O%2BuLQh9GBircA2%2BjSLq9F%2B%2BK%2F%2FEF%2B9pRQy6NV3ppSrGvE9vTZgCl0gizjqatpUkqmf%2FF21CejqtzVhhciZdAWylXW8ymk6kVV9gLOQJD21OR0mU4Fhg4bAtb6wrvxTXXD1AmN%2BSIL%2BZ%2Fqh1CdsaZXT%2BnIJg%2FnQm63KywXDjCkUzCtrylRpw1SiZtMKJvWT0jBPTrdweEqsfuISJDA3fv85AZbkpfT%2BfK1FkAj0oJg1edUn0tkAwv6Znv%2FXAj8CE09JQXEosqcHsN7I%2FNA9d1BJVKXbkir7wmtcaZSk%2FJ8XfWc66BabUagGKDqNyYuk6abNnV98Ixneik0nRY%2FydrQgZk2L2TtUpBRw1Ns%2FHW%2FQ%2Bv5FO6nS0bj716lMpdV0mjbNBTI7j5Rkit6kPjcFo77vUlBUL%2BMhoNbljEvW6kb6MSUOcMPwowNucqW3H6pPHiZ%2BJlg19vYOy4T5Y16YySkB4xLuIbITLIjIeY7OkJfkDqYXmSJimZVimBfh1MWIblp5o5STvQnv0k%2FmifNfE%2BPubZYSkaPUwOLn7dn0z%2FCxauZZcoY3g0sIvxfDSTRZ4uhmS5smRqpxYelJTOLcXS7xpJzgayHmJBhOMtiYDFUgYau4UJSNZ793yH7oGjs7zJDMRZTEhoMPz1rMYGOqUBFsVyuhM1xlgJaBnbAmapjJebr%2FNOTEN3GgSFe%2FzK149YVh2PGPZPFf1CtQCFXbMqo8oS8a0Cufe%2BvGEw00nitsidC1UQRZTn4QXa0N7qz4d9nADmTeeXAenvUnTGeZAsSJSYKhKwL1J5S9mucf%2Fo7YKLyap0VmjXVSKuEBrPQrz4i%2F%2F1xpJVPx5FyTJ3%2BDS%2BjokpB%2B3qcKUvSi%2B7SokGwegxEv%2Fi&X-Amz-Signature=6110d3a18306e835767776972a5e93f78038670f5ef47a337feacc237fa5d46d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


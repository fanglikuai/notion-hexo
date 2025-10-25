---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QKTU2PN%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T020050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC6LOv%2FZpPgDLuEFK%2BX0GCF6snRe0%2BK8eH4Dm0sYg4ywwIhAKr4%2BvtrLcGtWXwb4JksTCT0SE9cbb6jcr5SJuTXAB0aKv8DCGsQABoMNjM3NDIzMTgzODA1IgwJVe0BfdNcp1Xxpvcq3AOZhYWxX%2BDp%2FgOXZTbBSsX0WVPYvuduSQdEhGLJOuAwzHAfT4Xkxv%2BI%2FT%2BzZWA%2FmdWoonlwUPFAaWN9vETSlJvWbtKfkZvmiQczt13ulXIl%2F5IoKd%2FREd8moB7ULtNPBvp2d%2F74NRBZp77TQLnrsTd1cXvNQ5ZvOfFnEXu6N%2BXF75bDjwa895yzD7YdevlRZRkFR5kjpyooY%2Ffgd4LrzQ%2BM3QAJj76A7TV32d4siKJhCeCxAlHV16ysnrbZ%2BEYeL82dxO1c%2FdcOO3n%2FS66UxMg22oDx2VLmzUod7FPfPjDotnWG2ITaxHHuIsxLy8mn7BiP%2FdX1Ub4p%2FzZfnbLF56A1JBt%2FEZMJEE19Q7GLFAL1%2FSeLAEXh6%2FxbDoKAng64zc4B%2BANtrf%2F4G9NtMRfd5pLAyLZ0i5HSpfCU1USS6EYgrqTx7Ho9XkjX1aHCiF84GpWC1DIKBHPVA0k18xsSp4xHn9ldMJjiFUx80wkEgzoHqhPtruroPZUEZiQ7zYAULejt25sP9fNbxhe9KYM9amHB6CbilMNRGKbgYSGWYTyTatFlUaLDysm1HCI2QVyWiUS6F6ATwzhA6onPL%2BiUOmR7WlYRttOGaQacORUXJyhG%2BnOyYn7kYtJiYhmUMTDU0vDHBjqkAfmku5Vklt%2FVagXw6ptKLDX0ZWzsY5sGQyV3n1bnISKcJB%2BWChpO4HIFhAn9NT0dajpRoVt%2FIuJEBnBwQHUB8OyWGrVJEa8aB0gYxYvmPnynvtwn9nQaoYoZidUfjjGAyutPP0ozaSm9vjLoLrcyXmzeqEgQO%2BmJho%2BuFIKXnKTC8Kma9DY4ebe3Ub2mAN5bXjt%2FTFz%2FwIHC7V7hDga0SSHb0L%2Fp&X-Amz-Signature=109d10d78d02cb83ceac60f5299c86a17e63fb03cbff1aa94ddf30dce9c25045&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


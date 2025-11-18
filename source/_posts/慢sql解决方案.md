---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHLNGGDP%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T060135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCLD5VODVh2TWu1hCW4NYUSoft6SGJ4P9qXf2fTLU75ogIgNyLeYTYS318zEuARaydqJPbWGjrv4XNjYwcNqjNvdCIqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCwiwGsOQgakXBCJFircAzhodBJsK82E6rISN2yZPcENJHaDfjxZ1e6URsKGx%2BHaGmIAdhTet0CuQznJO3kzXS7WmMBhl6UgORa91JL1R6pTEekPktz%2B47%2Fh6hQ7gpkpioEb9Zm%2BBt1hgaORWN9%2Bq7S4QNJBGSlGB%2FnzGVzWXZOl7dZjvbki0nhafRcd7WCH%2FbJfJKQ5i1BCjqMAMFNyoRzBde8PlRtvn4k7moKUbIcH7I8lV6H7PY2SMqz6gC4HfUt4am%2FbBcmfd1yUk2QTAdBnkZYIEJjk2b5gSWV58CWS5Di5j49TIxTALvdB0VWvlrXPJz14h9BZnqJV8Eo4cT9za2cxjWnuwVIr0wAd%2FEcoqgTeAaZHjkAJqkQU8zik2WRXxQxheCGil8Cbmn01WLyqVDUebtAuBv%2BCxo8gxR%2B7ozePRDuydYVrvJKiunH6B1ebmdU0Kqo3JqZ35nUi246zZdHtYI%2F6lmNDC2fG2QrwHUU%2Bv8HPF3XDXo6ksVvlLtkFK9FlGnlVmn1F9cA9DThBaoggQdeGid4tA1KjEtbRICbdE4pmj2fKS1uN3I%2FYU7RnsHSHlUBx8i%2Ff0AGNz914cp06OzXAe3qLJ61mfrtI%2BaHXHl4Wjcrdd0PdZPpoW7Ex6B%2FyrfLZ%2BNG2MIL878gGOqUBLudDmQNhUhZIENVkpasY0w4wLXK0BhYDYl%2Bj6PEd45qhtJyoLMIGfZMehETBCzc%2FDWCM%2FxSo2LTH1se8tXjtGISJ1%2BZd8tolcB80iz5hioPMVKQZP3ncBUFOJKpdLy5hE9RmtjF%2BYKGLT%2BKzEk%2Fu0IMHZ%2FDDHYYFmeroItU29WpbRXJJ1KDhxZpY9B%2BJoPmitBb2iP3osYNjgr%2FU33tTmmuOCP0A&X-Amz-Signature=c5dd3ff56563119f18a6408efb694f29979056165954d3c742457538545f576d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


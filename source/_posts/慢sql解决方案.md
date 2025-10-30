---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7HH6LI2%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQC%2Fh%2F9d2ElSR2Duhg1JbxzzAdEqJ04A8WHf97m68a5EaAIgKwiOLIx8h8hery8bM%2FteY2NPpQ1PCdlx9IpjrGBV8aUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN1cqOGa3MofZyLDWSrcAwiqWcnZWQB%2FnssKhb0cHQUBVrp0a4I4mIHHzTZDbbUQrRnc7%2FRodQfixEv68s5qssFYdJOU7K8DyjsoESrqkWnCMdz2fYgAMxQJVscsGBbqOuUxm%2FhxAbHESJ1JLDrFuwtS%2FEns%2FdthObmtvh2RWzVSZuSzYcx0cSqf%2BjeWdZJFf4eBoD8ndhhbMHyJBlGPAlaxlkrrepcy4wgnOxb%2Bwllt3laMZsd3Z%2BuZUZZ%2FXNSKXIe8zLUN6guvgasxmrprMcax2KDjiKGxSVu5Y9f%2FO3tS6MzYKzwBMWMLchcMNIPAaiuVC3yenyZTD7vVb7AzOuwOiAB9jQORJSYWoPq%2FamGvArsYIiDJfP%2B9utreZEzZXXWG24UylqjbrpV5NuAPCDk8P0uarzhinIU7sl%2Fx0U%2FLS%2BHBsl7AsNbbNaJIaa59VR1dCnv2QOFFdes0KeJSslMqvIRF6QJohorgm%2FZ%2F3VYIftHo0qOAoI6V%2B1z%2B5N2C49t7KVTE5gPzFeuKvRzEzhBeo%2FqsHF0l6VStuYVgjLkE5tCXwcRUFfI85YeYeA1qnQx6rliaPP2KAQfggAHosNkDXVXIpkcM8KQBktVzwfe9JLACnkm%2FW5BR0NKJFuiY6kabZpSWp3qU6O8fMLSbjsgGOqUBsiQbUxwpZ1G7qSv%2BkN49cDcjNWdEQFQwK2LPfYT%2FvjK9lSOWPCFJ9Ps4444LTdewXtYfyx6J4X9nidOdDxn8rLaxNPf01LMc5HRSSvw4AW82UFmYAUBBtIaE8rooRXKmHhcub85mQmqTSqqiBz%2ByWmkmPIIW3fTr7Z9d0t1DopoGuFzb8eNiNjmXv9IXeltWrWf8UVwvloKtZU%2FT9x8iFsLKYaJk&X-Amz-Signature=ed59278a1868ef93518e6e19f82488a27717d7b61555654cc8e27f2b4a979105&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


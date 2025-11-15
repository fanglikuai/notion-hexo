---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KCKVFZ2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXN909XXCdUfvMSFzhQgIIntae6BO%2BYtQUHzzzDcRiNQIgU%2B4wM3q768qNKyuGk8m3KdG6zNCmnUsW%2F1idysiC1v8qiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDr%2FrqdR2r%2F1QFoyPCrcA48faWxo3CShfg73cI1F4J%2FBLhKJHZCYgFKFp4awkFXLkwS7k8X1NFFpbcrDVOXuq%2BF4e1bRlMyulzP%2Fs4tWvNhh78ya1hSbYW9Im9iEjGrX6PDje3%2Fez%2BwMpoGYAlk4of3Kh5sye%2FrTwM6V0rcPSh2YnsC25XuZbRfzV23mUugI7IbKfnQnNrpXQ2XNA0%2FujdI1JwOsXvw7RX%2B%2Fh3Vx3swTluwSoJ1S9KWGp7kK55lg7w4iU8u%2BgRgtATWh9RCyqzmj0%2FHFcPUWRrkRHJ1gCJz1xBM8MAnIaSf5Fi53wpr6H2NPYddvXgBDrhm4Edc%2FiBOWQIDSlu883BAYrRlIUVDqF8OvwTd7rf9wpZvlnC1QUxnGoFkQGsb%2FOUwtYSCkiHTnFuiw4DWMkH2vWl04JLcH6JoAdxG9R9VePgEw0hcAdV0QXDsGvs9ZxzQtxu0WOAkvLgtPpfZKPJN99DtQ57wwv8SYP2KGvPz0Zc97ISgfL6rZP6r2FonU7DIECJYj9Ht2cGvcj0GzyiCcAjHceIYtXGSWo6OH%2BxAc2eb%2BgDgJnQeF6eNOFNBHC0RdlnzOqJZchdyyiR374oWrpCXGrtYsaZJS%2FInaUWdynuQDs%2BU2LhCfvU5b11Kg4fw4MMyh4sgGOqUBJ95qP3bsMShWDRot0Xyurj0WVc4eNEsFarS5h6bfgBtyJEuavlvXATFfjlEi9s0CTOJAZwawMrf7xD7kOQTWnLE6QPo8EeqXwsI7qYr0vH4xgkf5Zdo2FAn7o0VfkKQ4fSNnrWQxKdyERJ96KigS7jEI5irhCM8IkDuRJKe0tbomrEffbOs57nNaS26gCrVrMsn6CHaxXXTDk%2BWakAjQwZGExcXG&X-Amz-Signature=79666c9837d92e6c95138333510b6bb32790ed3cb620bc73927cb9bc0b29df96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


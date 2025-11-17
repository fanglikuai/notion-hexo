---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JX4ATI4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCMD30GMCwfG37JqxVlUhsBF3jjMIvJxfh2WiGcozPphwIgJ1hGzxTVIApZy6Tnb%2BrUFGoyLGzvOzd4ILIDXEYLRKAqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD2yn3Li9KDSZvhMpyrcA42hK6H72MqzQSN1npIONkVXXydflfUxDLINe3me7nXV8Bn%2FU4e%2BrC3yfoc3em%2B9M5uK%2BJV%2FIiHYXLA8CURBO0aSg%2FyRxGQ7ICHZkE6Hk4aHOItdFzy2uY%2Fc7wr4MmKq118fvZ3i40F%2Fo8MPkFT4vbpnaiIiuceJJuRh4hXYiwdxO6J2SLuC6UHE0%2F3kN%2BqrCznkSq3V5br6mS8r%2F5YRtV8oLe%2FUIZDxrSgMG4toD3eh5OZgT%2FNWtK3m6B4r9ZLSJ9pFKH6qduFX7oG%2BANNVDJ32JuzIoyb8kKu3nhN2zWQ6nw2mbwoUb1ItOGyPDtQW5%2FKRjHazdRr0bG6SoB%2BxSg6Of3fAYS3rD3YIBZqnUW5VjMICwbCjM9vryA80rziTqoSu0vUlMPk5fi6Tj7ueADOcZJ1S6TM9lqdUBO%2BWuuQb1bgG7SHMe29p9OzEJhU5Gv4xK18seS02kC1Hj%2B8y6EU6heLjFjCoTM7QNoW7erKHjRBSAX3wNoYW9L%2BQMF96qO5wZcUHIOevt8XS5s94h5ynb07Q8W5FCfVtHJ0HrASid%2BCX%2F4ba99Q87%2FucPdy9McqFsZl%2BqCtX3iwUQyN87A6Em1azu7BpO6DCmpBsL7OtKWangyAlR7xBBwKaMJjj7cgGOqUBjZVRwY40i5Cs%2FyfRB8jT8f2a2HAPIV5nW1iRjlD%2F5FhzV%2F85Tf%2Fzkkf6lG32tuZDo0fC3eBy78mIKTl1riXNqdUKTUP9PFj6Y3dU8VTTe%2FOdtXTpomKm1eWa9Z8q6z2gufeWcauZaA%2FTmn16vBUFptrfAY04TtLWOm5uf9%2FTnN7yOXxhRda%2Ff8PLBuPbn1CYv3c9t%2FPexNEWqwVrl27zIjV0B4XV&X-Amz-Signature=63e6d591beda52d1c3f305a14bd66e71e096e8683cdbffe190940f2d8a2ec92a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UI6FGHQQ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIQCbSozs9Z6IwX2Fn%2FROK2e6dFMBUp4Dwop3Fl8gMBwC1wIgL%2BBX4JC%2FTfCCvmTmQqr%2BtmWOnPE5alNtQmU8rakk7poqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAAiwrVW5OssATMhqircA37cSch2YmflXu7budJAj7epq1KYAfYKdTIMd5eRvY8PTnV55Vov9McOi%2FvwbVXjUuCZQn%2Fj1E8O8ZqKeYNeZ91sAgo9qUbA77NGfs1r5nml4PuNFxM6FE9M10M0%2BKjAJwToY0pGl7uNIoh9bBBxWSEfS23cyJxygXSczSv%2B9ginF0VfBleCxvYpByZnupYyw5%2F%2FQxvmLpUbTZTkJfPZ91dlV5UakPGvYykglpU8FxoisKSkz18XciUE%2Bsp8RHCcrJeZFhFW5gMz%2BQgBHmp6qTeJdqTA1peSEMNUoUwXGLOjV4%2Fn3kpTFx2OgvKb4QjomD%2BzUTbcdcniF%2BzmEOug3cLMe3sqkhMxg9VWDFo6FYVQUSgk3awD6WCSqLHgpQwmlfbdHtuyoDSJ0jR5hTnJFQSlmxLOZkh5JQZ7naxp4vorn66kwl0Hrh5b0OndE2iaKmvnik1lD7w5U0aLKjoSwyWzybIpjSrNj%2FJoyP0%2BYU3MFqcozv9aQN7G44Tjv5L4zLU1suEv5XB5gahjrZ5Pg%2FJKWSV9pM2gZpQdiPQkKvrjpp9HY%2Bo89chyuE8U0iWtFzISHJMxf33We0jgEVcchuBbxduD4UUdLqxYCzEp1suXIjRUoT5Pp%2FpmdTfAMOv%2B%2FcgGOqUBUTyyJy%2FSRVHp4ScrtEMzt8pr%2FAPBJ8pUgnkU%2F1Wdh8t2JFdawqqLqsKwyYC0FKZayTHXvGiso2yAtzbzlvN80PBfG5Einnv0GEJLZJ94bIuO6w9qi81fcjP%2FekxQwJFp7y8zjbZUG9xQilNqmc5By%2FMXTdHIUK3TPBYe6zaVKtpsbOx7SxDtSYYgQgo7wnZwYDI9CApB4VaJO9qkqTuN4hDRJrZx&X-Amz-Signature=5706c900a472f11d23983a98755eca1222638c011ff2290d9fb74b7e1865b7dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


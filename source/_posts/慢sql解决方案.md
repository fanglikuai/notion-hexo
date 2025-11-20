---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVKOV62V%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCIHlC9XBvuYVocBvrpTBp3q%2F%2BQ%2Bp%2FKEuCSikG2JtbAW2LAiBaPVvaAFtBSzIaS7D1IgCHNy4AOJpDbsAJat46fIHqxCqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAK9t4cVQBTGh4nf4KtwD%2Bx6dZ3GR%2BnM9HR7scwNYKhY9iJ0RVuy6XpNEUTouimEp%2FrfFT8%2FXSmuuP5LlBtb8xaRgwlkI3p0hJPGz8eeg%2BYMtm4KWeHkSIn9c4thkDZgb0AuxiW8UouSDBIpPinsQ48fBI6ph%2BsgBiyKbqyudnsLwGqgeJzVixIeQ8l3jopQqjZk2Es62DsS%2BuxvkjhzQGqnKAOkEF5aJffzJNC2clnCAWUXBQkhCYWiKpz3rpE1SJIdA9xDOKrHJRfqk1eYeanvBfkenLoaP7NDnEX7GErn%2F98ZbbpgjnsMpfPJjtaxwiU2UnSoo5J6Rv2KLunhPKuq99xzrO%2FcR5N2ZVIOqFlOdsMwkI3KnWpyaskLnF7%2FEoj8OlrxLE3Q1FFT7v7VjLsD%2B0J5SO33%2BX7fFOJwyCKpaB6weS8YznoXoXeIP664zXhj9Av1V821y2FMZKb%2BoVN1JcQlhoRca7wUgMqjfO0fTq5pV5ZbGRed%2FyOlxPYro93mx88heIjQ0YihQtuTASCqTBc%2FrIcrjo2sXb4TUhQP8R21iHWGivRLr4TyeSGfJia3gaBQ2aG2SxQA4Wbi9r6GgzNmsq0uNMLyQCOfGVUlstunkPJiChUi%2F6lBhB6FWZrcSVdlUxAaVUOcwgqn%2ByAY6pgFfBzd79MD8Md%2BbVE%2F8BummXldFDOImjO%2FFp7O47lNyZZcgzxgynOzYyBjg3k8EsCFZshaPT60Y8FP8hNvGgmc5BveXmK6bQ413JVsJ9JWeBG%2F9bfk4DP3%2FAkbBMjS6hHmOGzrwbzaPjLXa3Fx9ZJivZLVL%2BrihkFLwkhPsLYOiUObWfnSlgosTk5V1NgjK6eAAxkJaj1EtBa91IqjRUDM23Ii29m26&X-Amz-Signature=d8945b633636d4e7c5428d9df1956c53a68bc27f5cddb0e075ff8aa6b6119f64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


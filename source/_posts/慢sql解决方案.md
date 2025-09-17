---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CLSP523%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIHouFWs7umBK63iXbBqg%2B5KvlSkZGpbfkZ1MmSU9olJZAiB5I0JZsldDEzFfizhYkWr%2Fth%2F6gNwpSKIjiS0sjL9Y%2FSqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrVcCzYHlsNRILIzuKtwD07GN%2BukEjFY0toM%2BLoZ7xOyKSyV3nvbxaKky%2Be1gDfEs4ZEknitgDhtuA7tE%2F0rzNUP69ctUOQxfWl%2Bkib4qvdga66Cjk5rhGPo4Db3AYgufI3pMkzMwilvrRQUQuewJFe4DG%2B9LRQji23BYt%2FwoNh%2Bogwg61EYH0VkQ9XtgpO8n1oJUJom4%2F84mhflS3vzsdvgf24nhhOKmg%2F50FYxoDPfVPwlSFtsYTF%2FGy7COzB1BUjlpz%2FbPheWTv3wUFnJhWrtAMJXpOBGMzlPdyzr91bodcIbOEY%2F0LcFFh9w1ePFdwJJQSwMmBfxJULp1jOsKUiEllWq%2BRhPSifEwICi%2BbY7yC%2BrYrtWWDTmaXbhBThXvgXscQdJAVVLII0RL0bLpm6jALQli5NSjx2a9d1sY8AsAjU%2BJ6tbB1dA%2FOL7J0l6cGQX81WcRtyoSWkRqgKmJM422aDnrScqP%2BwmHF19a%2FuVFcdyucKqYdANVpDCnYUbb7cOQ5Ibwl8lpJZ4WVvCNfqg8CtsaMQe0cMrg%2F3%2FOJpvcFjN6AHe90DZaAvdGYF7y9lI1qFZGx5zGQwTbNH%2BhIxZjHwWP6ZeTVjKxLuhS6O5Hev39R%2BlV%2FaQYIrLXsMlbuSobV8FjLh7qd8kws9ynxgY6pgEWRBPQy9jUlZI1F2x4yyJD6xR%2FjdcJRFrZat1GUeZ7gr60aMP4NkQFdqKRQYXGpttzJJWkjkamvWF%2FyQD2q2u32hAQJgpLJKgt3wQDY1fyrcF7lQmykvDe%2F3U%2FeieKFvLO9h6JFgL9Wsd2YyoVH0VmOFPG0%2BeWOrrEQ7EDVUmmfAGKhxpgVTWGushPFg59l5yw3UvJe81RWFKy7oMDqMpW0nTa0I49&X-Amz-Signature=6aec2be6ab03d28442ac3130ccbf924b55579a43ecc13a68a32d1de7256f93cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


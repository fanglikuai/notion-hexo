---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RHRLAX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIEUuZilhdogSr6D1mVZlKJe1KEf%2BJKuxaJIFDuQu1fMuAiB5G%2Fsasfcny%2BTvAsdJ4jS1BUbdwesm%2Fk1dkb1SOUvr4CqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPQnYaev9BdhW8D7hKtwDcjvvAiZDiyYbAp2zyxQ921jU6LylnXrxhSoz2o4ZyPxLaEN4x8polasjF4xcCryfsq7ndRYDjjz0oyTBIAn1EQZ3SDHUjbW4JJZplU9dDf0%2B8HY1WHvOHrr7l6BTE%2FV%2FPuH8xPFB02egzu2AMQb47DGtI1SOfbJeJCoROXXYuKE%2FkmcQIG%2F9TuhXzuTzvkJXtE78aJKDgfKiJ1VE8SnAZBmDjCfmu9MvCcZff%2Btd2h%2BX9ZaxTPzuGi3U%2BhRwz%2FwE1TQuSbTWhEw48HAH7%2Bk%2BlcqDGNiQ6Z0twQ3H%2Fwo3wXQZMGAtv8xQjOSD5sBWgq%2BdZQ3KBlUuFVjWTRrAoZP9YEfll7nnq1f02YYRqSs5ssyYdF0aXaaXf8aNWD0ZevXi%2F3OS8BuHeskZSp0GYZp5tO%2BW1EM3urfAJazLveL%2FGpwtNNjYgmBy15i9kl0AyOLHSYkpgjTlpXlbqsVNHU07LI0PEdYRAxBrKyniaAaLNM%2F2%2F6%2BVr7HT1H0YJMLQtcPBHu%2F3Rn%2F%2F56cEjL6xWyA6aeKN0PYBA0BHMAGZRh91DqdGuxZkQSuGWcMAStxl8g45xW3beAU7pZmJBi1S2JWZkWH%2FwowIGTQfwvBQF3QX3xxUFw795J0lYSg6YHAwmdfUxwY6pgH5jayAeBrVEGSvn541Lmdl%2BN%2BWtAbPZDu0S%2FsBwXIIfVA1pHsPQ2NF3pmYBZaYUpS%2F2ZLnycZAK8d0Af0mE7G5C5jIchi%2FYw6JQ7OdJfpGZZPpD8wjCSTJvUDTOJWQp5pO2dg%2Bktw7dl9jJcS0j4b5hoYvRr9imTYXlO20AySpM1LjmInJN2g8uNYZ60QUl11f49E5y8K%2BGbppYOQk22p%2FXczzmea7&X-Amz-Signature=a93807bdb8b70bcec7ff5236cf2e8fd9f39a2a2179ff961a3b4b96097aade90f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


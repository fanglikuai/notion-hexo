---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNPG66P%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T020003Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjZ33VKvxapVpUM3%2F39%2BEAwzlNdAWkv2Tdkmzc5Xi0RgIhAIljVwU1npn2GHmJOwvDzK9GmHi2J34z26c%2BXi%2Fmp9i%2BKv8DCGgQABoMNjM3NDIzMTgzODA1IgzEFsj%2FJ2Z6giPH1nkq3APMMOU6x8ujvqxyQXo6fcgO%2FUp9XaZcxFRiWJgC%2BaoIxiIaJ7YM8aRUp9hlFMixC0iHn8p1HlpfVqopmQ3KTNlCHLgrbMR6VpE3q0rHKIUDuviNUlTvNwZ1Av8M7f%2BHaKfeokXCzYZLVdFmolrxJFhdvpySrICnMHqGw1CJElanSvd0%2FqMCwKGogc3s1cgkzyCuovxa0Zr3jULKWP9hWmDFP44tErJUt4bB%2FpGl3bEJdNlBdfq5pnPkoo4W5a5V%2F8p9Wlr9B5IWHjpzb4kFFo0jT8429jMTIfXj7lnAXZq0cGLjsM5eSD2BJx2F6LHb5u7K63%2FBeRf7nBtgPrVH%2FE3IKDtYFLXnr3BCZKDqC0pkC36IOwR6Ep4gZH8K1nwWxssZEaCw5pnLFVr23rpIk%2FWumGWP04fc2G9xJABT5myzG4rpoWzJcivEaUKi1PkY8oCkqGFtf5M02NRKZS1I6hBK8sPj%2FHxLKpYSzmzU5PAZWv713orxTwll8QV9x8bgJhDNU8kWIL62%2FyKu57552fe4akbQ3qnHYMy91OQw6yS4xI%2BF8bBxI6kY4LYD7InWma2A%2Ftv4FZdFbat4kZpWE27ApkJ%2BwcBHM8%2FVbsgA5iw5p0CQbbVvj%2FHbRygqaTDsnJ3GBjqkAfgRo1xKZ9%2FSEj1%2B22IA5YjsA5IEO5mohB%2BdzyaQGKAqNTQtElsOyQ1EkNin%2BFrIwdJf8EAYsyIwRU9qV%2FeNR0cLcwktg1MCkg3X1sKSZYPt1DLme5M%2BMTykQYUo8AyAVmuW1DrngQQ7GkKnScowh%2FPGRkLYn7l%2F9m5Nez3O7vDS4E%2B7UpTKYbD6A9wWbiyzEY%2BhR8BhuziNQvlabwzZkmWBGWTr&X-Amz-Signature=130296b5a47f190b49f4dfd5fa59275b82946acf7f102e72a54f304d41afbbe9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


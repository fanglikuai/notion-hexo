---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIKVY5HG%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T080110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCusF4A8DahVLko8qltw66InqYDeIaoAGIWxgf6iT0t%2BAIgVUxgMHlNHbRUqos%2FLDzn7Qo0GC1jg7%2BStO5tBAMeStYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB7eHeT4wf71YDDCkCrcA6deQAlvS8s%2BmjyGYM3etwBITw5ORyhMsoJTnXuQ%2Bw7sbNRlp%2BG1EMPYgP%2BmNCGxyzfdoY22%2B0bLez0yckFDNO2mQw%2B%2FoaAMEWopeaJtrfr2hEsdeCX8TfVYe%2BCZ8fCsQifScxeb6ox7%2Bu6nq%2BtJXFoCQZ97wxK4jkaJJ6dd%2BWkXI111yitr1kNz7hMfNtEkXf%2BIjEP7xALIdG5Oute2p6E%2BBh4%2BkN8wnqiyYgXFN7utWquy%2BksmYlOExWcqtDVDjJWGh%2BSUsSZMKkU5yH0HC3eKlRUgDvCWTlj4rXH2hcXZn4r%2BJmkBfcGizG9o9xMPReRfy1FZrcTo795iloDvrnB7UkQ4nZlVXUY1fi6CxG1pQDi8HnKYoCoHg5T4jboeRxooZnWjYpaDeqJ%2FPhR5Fed%2B8IsF1Q8%2BzNjFXWbaNC8VAdlTLEBeB4ZRr9qgyNRlGf0qoctTcypR5h0sk%2Fmz%2BZTzRsnjLfyeFxwC1Khcyp223%2BrkNMZh0lt3vxtdf92gX5JfA1kKJ4a4G53Zqt9LxSZSashXw5k6Zr7rn5bpVUMUprRl4XlxBSmI3qEuVbYkB5XvghZG8Kg%2BcXQ85uJ3Bt9G0nluSy%2FTEJkTGf9Qo8Aw0YA0iIR9AW6QjreQMN%2F%2BvcYGOqUBF9Q9L5WpAIkGhXkTbhubjVbK6%2BUgEBm4Rw5B9YzWSfSYO2ne485bWFVb2UWqhhLuo8qWY5UobFHav3XeAUC5PXjzS1iK43cCjdYh%2BojUUxXqT42NdNI6FzHA%2Fs2gaasnKqISU6dzGqTv9VpdQpA8NliQUkX%2FhVGRzXkRGe13RQntUHklM1o1x3AhnMVDxKHcYY2gr9AtOhf0fJyF5jOOsCS%2FXebT&X-Amz-Signature=d3a74108eae1ea582c86d0e9829062ca4aa7af39c4c7a4de56204b0e7762ac0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


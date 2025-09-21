---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKK4T7KD%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE7PBIQyzfQ4ok25InepgAgVEqFvWqmgHhMI2CmNUZieAiB1FLSzZpsmg9LvpubdeWLTVQ9EQatfbitlOJ%2FbiRh7OSqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6FQJnC22ooVOKUweKtwDz%2BHjMRuzOVJW%2BWNzbUr%2BZDoylTVIC0z91TJOUvxzOeo2VspEs4ZRljRauv1TBgTVnzbPEGhF6I2m58qm4GsKAwpXVnyyMuUkAG6Flh11AQoM196x3ln98fySFl0oCx%2BzQYkobEm5whmqMmdrkK62XrE0I9JrJ8Y5glf24UtpW9%2FeShvKNGpAsYjS5ZzqG61SZpP9M6sLM7UZ5oNGiPEaPcFnwexoVlqxgaZNXJfEG%2F3%2FNUjXnn1q6Z0WBts4JjdP9nvdsr8QxqlnhTqZ%2BpG6k5%2BC%2FkxDoE28De6MPY1X59DAeR3XrJaGGa8NAG%2BnnsbxlD96DQv1eebVsTT7ybdDZif6QLOxMTGGVKqIRLKY7qEeX4QZIiNvkD59TJVzeR5ImMFDcuyAiw2tUnFgdDrGuosdjOqebep%2BXMNFTSm43FGHkwZuiTHH9oHL9iNuDuuc72pd6KLHAsruqkb4GxcH0m%2Bby9f1B6l9Ct5yUdvvq0R8YrlMBdUrOTSqsxl4y5T9y6EqSeItIAzqrWfiSyHrwAddKqNsYHzGOS%2Bn9OGMYxJrlA0R7gU88rZe9uCxQfunGBTnE4wJq%2BZ%2FTe59KG8ZbLVNE%2BXtd5lBbQMuaRkZDrzKXAE51kSTp0V%2Fwbgw%2FIq9xgY6pgEmX4XhSLU36CXXy9LfMUPkrCfHUsnfO08zvhWCuIvzpDJnHZcmcCIPIbCN9TcululF9Kt%2B%2FnP4URgHgVh1WWMRqCP%2BWFQxhw6XRqF%2Ba80Ssc9PBvBceu%2F%2FJFGo99xogNvzpox5fwEttuFTh2rJcwCs5GO5B519I71nWZ%2B%2BHWGumU3K75B2A7uPQWTPbu2Qf8lXpGfOpxCoXsGANzRuYwgyuFN%2Ftn3Y&X-Amz-Signature=0fcb4e63d593421302cd15b9566a43dd3a6f7fe1add54e8c4bcecd4dfcedd24c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


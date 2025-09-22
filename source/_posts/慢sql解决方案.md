---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNVJCRH5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T010102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2HcaweTAijwL1ShJzw6DRM6Mm0iXYPK3LfQTi9uKVIAiA02yZsQHcHJxzUtZ3UfQOOatduHlT9kQMVx0yhGvf3Oyr%2FAwgiEAAaDDYzNzQyMzE4MzgwNSIM%2BtGsvurEC4x4RfMFKtwDhOOB1sQorNIMdchsFxojYqWPzi%2Bfq0QTuLRK8Ml8FplQmAOZv3iRp1OrOAKBIgvTQOdDq6hF2jxG%2Bgr9t1rqoWakGgwznh%2F6ycMEQovfrv21%2FiLhq2uw53isjoAjCXyfikhbIMMemlw1cG1U0zTLheKBnaTIGFPqt00fv5GFyKs4w1f6Tp1yQyP%2F4X7GVKxCW%2FNpLZHLRspPceIUp7tgpPcDSbDJhR7rQe9UC7B1J%2FqGYZVVo4TP%2BHb0wYNu8efWTA%2BF8J9BkSDNCkhSeljsIWH8H5UUVdmXxAuzITocRrTeMu43ZlEWK5JYXyeQNVuBbwKr4u39KjGPyI5jkfjHDzTmg5BgxViExsTW2n6tllmL1Wzy7d33aK%2FLkoKFsb0xJQoc7ADkZprGpq97dinm%2ByqBl%2FjaGKbxPYe7NIbUpfEirakCR%2BbR9yiaks1h%2FZME%2BHEdPrx5lE%2FGg%2Fs7%2BTSWsY%2BkOE7i2cin9cLGV%2BML3kIk6i3G6UcnSBZi0K%2FCEroJPQU%2Fe10xhaQCyKeYaX7t53%2BlEquxZefBQNu0Vqa3NQ9gFj8BLVnk7nOd4fj1etRQ9e%2FQpa9vPdYwvSDaMz7ftrYj8o%2BRJ9Vil0IuENLLCIh24wbxH2wGS3OPivEwn6%2FCxgY6pgFER39B2dHiT%2FArKxc4%2BNILMHGU8DYHUO3vT5ypO18A%2BeAf11NRouc6WeF%2Bjf8eEvu5aXSfIS1YXN%2BQ4Ym5rKZHVSp6KqS%2FtvvM3%2FRSzyfGoL9lKxgk5Gi%2BS6SqiPXJZ9rO3R%2FcFM%2BBE0SwUIw70oaxi38CSGQ%2BbegjudvD64LwimJtQoV%2FbOEA2BDjaT9jQJowFUCbj36SOT3Ad4GTcw2JjTnqr3yQ&X-Amz-Signature=10ecac478b9f39decee54ec4cbfa3414a1b37f72fd138dc67394b2cccdef30d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


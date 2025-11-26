---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626WQUO3V%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDJhZdrpbSVrlC9T%2BuilwkFo2CenQlVR7VGmJX11gQb2AiEAvxwCqSfJUj8sUHOX1oryIu2PFrwAvdW8hnlkWTx2bHoqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBVWtVCAi%2BMod2P%2FWircAypZuy%2BQpUGLuqIgIxjaWxDvIVdbDvDjEK5p5jCuYfep9dhiItyjnDePsfHJqVNLAxbGw3OrYqvqT7nBYfnzqZ7atdlEik22GBXPlVQyKuGq4YozXJBCWldePienOwfQa5gdxpVmVcnGbwo81EpFqApjoQ2r%2FJb5fNqMrcksGArNOMdAvSSZBt9h0DKDBI%2BjSO8PKtNNox34u0It0qqqm4EXRDJfxz4t4TU7g7DozKpwASWRJGAxDIRx0ojR53IVSfZaDPNhm1k6j68j1JobuMN%2Fo8p89XlwQ%2FAqcxwzV%2FBM7UXaIIt2ZhIIQX%2Bizy8YoIXWl5yHi6V%2F7OuyglN4gvrbDVoxUv0OucDHt%2F8H8%2FaSihXfNF%2Fxczuuiekque%2Brh3xxC1ZHYDw6CHUBohC7u6Jedn2GBsoFYIB4o4nE3Ij%2F4S460%2F1WYobuIQOJetX%2FCL9qeVxlZdqDja8%2B9qc1DT4fdSFjSlZhp980LgUekjdKY4BVZxrsVYNGFtiKMODeLQh9OngzXr%2BTPIY3you%2Bdi0iLld0Snyqg51Db7WU9z8dIjvUQSXqeiItTHiDrQcpYwuWPNoRVtGhAg3DRQb8qWdlB8JHXxE%2F254mLhQWQhzRyvE8g4wo90Se1FOVMM3onckGOqUBnCEMZcYHHcbLNehl%2FHdvSMCyOiT8JtgzevSoyPCe69yTpz%2FBZunSftJiLcFahxnOsojI%2BqcltDmgE%2BmTm2Q9c1y75txqu9xSdicTc%2BrPHv11AA6XKa1IJpSA6LkKg2YN2bWNWKCkOedIVXeOp0jBLWDhbVg%2BpsVE%2BJjmuQ9joOLWUR1eEWexpiDYmY5DDVSnI1zK7hMbojlrllZPszJUcRlqoC32&X-Amz-Signature=a552c4fefdfd9bef2c51008cd52260af002eb252508b63025a576a852f466a9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


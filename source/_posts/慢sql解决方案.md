---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V76YQ4M5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEMBaONppG%2FPxJvoumXemSgBFjJJ7gIGmIIhH4NSII69AiEA%2Bm3mwU6Oe7Mlf8XRNiqeETF7J5dNOMeCLkl%2FV9dDmqoq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDKoPzYz2ol14n9%2FO7ircAz8VrUXAA8%2FCgt4TckUWdJ8wdl3KV18bc4EbZw483uykzGbZz6q%2FJ7UtIfEVTdQYellcoDRMmcFw%2FYjwgMwFqNKg3cYy8FjovExWtfNhn1CvA4ZNJYy9S5etOXmJ8BwJ5gsOs1sjQJGBlqxVFjMnExqIXR298E8K2IJKRByT65BE%2FzMvKvawKdFAJi%2BTV%2BwEk8JSzxQ%2Ff40%2FLwJK2tmAhZL43QMkL4XqSoWLa1su8oZ7R7Nd2MFpOMyTFI%2BWHxsYolbuBPPKWwAuZVAb6IOioIH6aDE8%2B%2FBOjUlkLpZA3jgYVkqVW5znwGc5fE3OqH3r2cfHAX9EA8JbcLdihIJQJsfxV1KJIkLaMeDUqNBxFIvMVovbBwW5JSJnd79n8cb95RkjM7Grgl1da8I6RNSjxIK2ziXbmR2WxKoaZ3QV%2BWjk4TvK9dkSsie8qGY44Q7yeqd6Mug%2FFVc6UHZ%2F5qx4v%2FuwWuRbP%2F7lCCfUG06YxN0AZv9REAO3ghQ2brV3iuLOQNoEAIIzPix2K5Anb0fCXgIulHHE38lWCAp2otK%2BUh7vCHIKyQshh5nZRBUO7PFdLhWvkD3lHCFv0ILjTBa2K47D1mXylRwovHkebtZ9gbvSN4huUMPT3lsGh2SGMO7owMYGOqUB57ZkNgckkbJNFoksFaD1sSKfv7jW%2B3XLoDbCLIwhd6flGfxHFyLHI69kRLFf%2BkDG%2B9KXeC5fkufGP%2Flr9gHyVnhrVEYO4%2F21yBMH9VV4zFkTPx7e6V2SP%2FOwsBEPsnNLUNpd2iYvWJgkI49%2F3o9daxUYul04%2BKfdg58SZNF8CO%2FYYj6tFGHHZ0h9IOhkSHWwdEMLvjOqpj00INJHtI9DxCzYATyt&X-Amz-Signature=f578d409aeaa3b9f93ef42ad83e9d0bf647f82bb97298cf6316bff52cf845309&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


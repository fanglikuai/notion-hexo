---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBSVVRMC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGgD%2F5HPmUlfXcEn%2FbeSkg9Vvq%2F2LLjIK54HYhSBV83QIhAOnmZ1w7%2Bh84pWicvHxEeVyaUHQPIjBAKp2bdoGYpNMYKv8DCEQQABoMNjM3NDIzMTgzODA1IgwP%2FBZCmXtFF3AKmDMq3AMMhBxgZ2seESBKKZ%2BjXul6HfWdAuhBcFh5XTjS0wGUKCL1d0xQfDH0Jm2S567Pk2M9hJUJXv1wcMCrmkE5b2mNaUV3Rt6UduZNB6qi8Ha1w4j9hkR7BcEBf85XO3uhaLnl5pGWlT9AVEY9MEhTnahLoMTvrtaM5Gu%2FkmGoHxsCwMiDV3FRYBRdHVUSzEFedkr10PLeQ2DUHGXRNv15GjSNB27f60gNMAfEBFqGX7U%2BltGMSIW%2F8udB59Rg75bkVz3ZMpMa2quz6Cqu8HThvwmIcK9qgAK4HudSOmmwPhikMXdoAX8ZftyLG5KZcW8yPyCIXP9nM7H5%2BFyv1Y73mFvW2WnNgcWaJoUEnT6nkPwvt%2FXccu%2FwALdzN7ps6vY6jerdgLXfQXWfo0Qk31vwGR%2FrzyHS9GBxfeOiE34QlIfiM0ZdQ4VJUnlTiE78ZkO6wsVmT4Wv9sBrHXEnBeDErSfb6MeyvzvehaMqxpaM3Mov2rxLVUB4pEXKpC5tHYSL%2F2Z1ScK%2BbIp0Su2DGNdDgcVEl6mbCZAw555vKGIKSuHi1qjElpjScshylk58QNFG7rY2C0pXRcnSBTCN5wLngCebPYmP2HTO0%2BKFqED1yFOln3qF4IRVbQ5fWwBaDjD8pbPHBjqkAZ%2BA1FalGNhvK3467mOlWnU4CpHGhOzReI2vXnA8ve0oIkZkSGEXpPYHFZaHRVflOLhJaQ1Ys3kvIrotifkwN6GeUSpfpcFIMKxXwpOBIfvoDhkrVUiQ2WX94FbEHBi4F6rgjqKObM9ntMVZUs3HO9hpOZJPftynRRmwTUSnUBJjMq8xSugBFjifkx9q%2FKLW9xPsYspq1mcg%2FHx53%2B2XULpdGNtJ&X-Amz-Signature=9ae5436a69dfb8eee8aa666c1521f7ef93897f25350c6dcd818cb9811cd964d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


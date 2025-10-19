---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653UZ5TNX%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIDj3PZsKH7GZfOSQVkLmDubV%2BqmofTVjdthmlk1kr9rJAiADx%2FxakKIfnQT9iKqRluEYbuC%2FMYIzGIKr7yQzy1xHcyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlwisFd0BoQ2zh2VQKtwDA0xCmk35EWJghPSos%2F5%2FXRo5rQlwz%2Fsj8XPvow97PiGXyp7HkmszNSTc4mMt68ArMYaTWlrceuCRsA02KIaQ70g89zeS6t6pVK1B2akhmw37MzCpIR9VaA9%2FcF3MRsgPipWPVv1OrOcs2AUIk9p%2FphVJpwOVU3I2wlexsKeIU5bkjSU29uLakiyslFF5Uh6oLrSAwwF6MqS9GjyxcnSSqm45ubJHVO%2B6M3a8v8C4w79gf%2BkyCvr9E%2Fx5mqHQBVtGzPGedpzTtwUBPNPaoPH7NPLbyDsZDZ%2BIOnWwzzT9xbO7TQfy547jmsnjXI2yemlfHWhsOGfbtL3h%2FtYJhROmmbjuYwuSmjvcachg7QAG%2B9zNV%2BXBgYDriewtnPwr68BGyJiYXKOKipvw7wAssFvp2CxyCvqrnTnOddZKKsB4sePyQuc1NaJWQHCJOx5gkOXG%2FiiVsmyKKEEI4POba6c6Dt%2BdUqqYMudSIekLGofDlF26xeYpWma0i8SneOVLFNdIRsHYgbPZnrJBMGgQBghEwayXKijtxfQT44F%2BiSYeV5k5x4bY5z4GqKt0LUvT5U8jdxvncDty8KhX%2FJLMxT%2F7j0i6PEWZslvYszVkZYp0B2VQXiLcPBHkZZ%2Ba6EIw2OfQxwY6pgHg%2Bp%2BO%2B9Y7QaY3d9N3%2FhUDS16SyqalS13Gvxk61tTNPiyDFZSHPghf%2BWLT%2Fr85STMTxRtxp6hr794XQ7Ri8EbUoYMTMP%2FQa1Q4uR%2Bt6INRM3E9Fp%2FHlxbOC5pyLR%2FxZkML5UxRsxyCrhSlXZkJgd2AgAEZCzjKJAvdVg%2FhoB4sLMGmZc4wcz%2FUI3ten7GkqrIX7Ar%2BGiEjdnSTLABBbnYTdnD2HAZ%2F&X-Amz-Signature=7f9ade0675b78349ff4687407245ae4962f4c8532773110c50c67945ca7bd5ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


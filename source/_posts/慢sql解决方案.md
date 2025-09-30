---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLSKNI7%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQD2fL%2Fk6WlWlWvqc%2B6b4p377RaafZVkoTJaYOtL0T1eNgIgGBSyPQoJa%2BriCGcz7xRzs1Es0zTPqsiBSgPc5ddzX04qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMqw%2BoQ%2FutcLPOkUyrcA8L3EujGp9QfnNbCmKe7ME4JUh0u4eDg8Z9hAP7wQ%2FOiK99wqj2R8SYsqDCuK%2FGlSwB126Y47HOrdYA7RgvMswPxiWNWjKPiqeXBC9HH6bnv9YwkFrLaFi%2BbQDGTfA6G%2BORa9CUvq5ch%2FbYDyncoY%2FmrkgRHfkLtUs7ntHiTeHw3eG1vopiVAOruzwswpan4o253aLEjWKcI53RUX0hKU%2FLiBQCCmaOGuWTX9LONAT0S16jJQvksTGq0Lm2KzJuwrLDq0gKUojPYklMjUvzt6s3Cr4kzauSgUzhDs5fJv6hUew1FAe20R1dJCzvqLjZgjc0k08Wo0uE1RYLrV94ZdI8Sw82%2Fn1rhM4BEkS1aD7hmm4xpVxYMIBaVb6zpEwhQJxfUpySve%2F9wgJ7%2F6cVcOMv7PLl4H3A1U%2BEGBhJZuFmVe%2BVHl18d%2FFKiSXAHsBWKKw8DM2u60KjIgp4K70RGbkWHqrI9%2FoMnS7xcJEuPZu9o3av%2F3ftgLkwn0XyJDG%2Bk%2BlYzJajS4QdnM6i6oTg0WvA3OKh%2BQTQzhB6lLmw77ZOL5vG5HvQPkpd6%2FA8VDDPYUbt7fLZUGTiUJJfEZSSa9vP3yEQik29abCNxeFdFaNNVcqmLMTsIIOahjcWVMJeX8MYGOqUBBLNu1peD%2FdL5xdHKLUz8rMC3vJ5K3n%2Fp%2Byps7j0Lv16y9X7aUGuKxYix81yzWNHcPhqZ37PksWlIGOTIOhZDEcrUqJ9CLbj0pohSycmM0pX6aFiGbjvol6KB6h%2FL6OQL45oQApLhBDpOktqkbGIEXVi6Zu6EIbEwjXOepxjZ2468aH4FgpOBAFIhvLFLKvFxYGLq4NrB5z1wnVWd%2BvaBMAFKZ41I&X-Amz-Signature=9ae94788af6444a7dc25ce51a14b5808f33ba910c2c31052c98192d865201014&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


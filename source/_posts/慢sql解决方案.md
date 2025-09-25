---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEC75CB3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGoAaE10S6AryU0VW3O2qGE34sedxXClT%2BkkCOGeiJftAiEA6ff5%2BGL0IWL%2BGmkIqpMZLS005qKpBb4XeDZf6BkrgHIq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGDbOxXZQmb1Fy6RRyrcAxrrq%2FCGFdwHx7%2BY9fQ%2F4h%2BdLygxRhOk1gD%2FpWUT6ooZfcVUWSLkC2btQoesOOGV316LyC6cNZRpxouyXrx2X2xtvj1uyUL%2BlC9LW2OP%2FqbOfuO9zpibqz5cmf7wZjyQSC7vO3l%2BHToRVrqn%2BJTqa1jBIEy9ClAnvEfwItHhbwZ4LaiPYk89HbuNh93ZirHez5Kso0kG%2BH7PW8m7Gdy9QfV5UjK5wCaJdZWCE%2FHvSlDrGFIzFBqrJy111JxI0f0vdt0yLobmdAarBWsIwrVarhT4qszApLEMPTa84RKpzeUWGo4VJqJZ3ymIDebPVn%2FlWJLuPnB0KT%2B2enlAc9fdOA3siRrRNZCnqxecAM9K7%2B6X8FkDfYmEqyVpM5rJWtY1O9n8pqkYMpxPOEAEHqCDAysLyjUKvquJ8rcyaxN6WhldxejxxP1V2gMdnZrjJSYqdBK5De8jnRAsI24ZaOw8oJNLZZpC%2BZIqs4NPinf80%2BvS4Wb%2F2mXD2%2BUl4RS7uAKrV7s3e1CHorfbCche%2BGjTcfY2mOGQsz%2FdHxHehTEZdOWovxQ0vfmpo%2F34Xg6OzvVMDpyomg6u%2F6Xi5wXcqgQpvcIqoHJBQXJijizvwHMvqoJd9l4Cv2GNAIhWTFogMM311sYGOqUB9mIAYbvl3gqZR3OUfjEkX%2FQ9hgW9S%2BLYFWBxxbZlicVmmRlQwVkJ7ViJPro7%2FnRogSCncap5tcNq3J07Rg8HBcLnhNED%2FYf4TMGL%2BVFJAtT%2BOZAux2AV8L4tmdsv%2BzWiLt2djoVoctf6nSu2NHgjaHD3wxqoW0%2BFUq8J9JFaCNmltx5P8k0gizdUQjwM0Lty2VPyy0GbSWPpbtEXdM19NVjUe28y&X-Amz-Signature=4a96cfee5db1eface51b799fa1cc561ce78d5e03203ca7a07e59e970fdd19638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


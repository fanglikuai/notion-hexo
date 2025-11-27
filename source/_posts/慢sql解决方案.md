---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDKPSLII%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICzwbPfHJMbgexgiBJyFvrtdAeadPlvmP8sQYfeWjUaVAiBfg17RwMEgZvEr%2FZHq7NxAyRRfOQ9ObsugsMg7E07QiCqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjkk42f8LP18mC5fYKtwDhtsYSe5%2BI7777XQMRQPQJ4UaTYLDV6DvuNvHnYarROuGe2uI2Elk%2F%2Bq4smb3NatSq8mRfvKwmfwk0BTFSjygyT4lGf1IRETXwmI899m5K8Y7vcFFRpnt%2BGCRdu5hz6%2FaM8p1E9KamjU1xVyKiRIgG1nakEuPPJIQkEzBdX%2F3PsU2fIE1omTtFw%2FyZtwcLvXlhBB5vM%2BELJH5%2BWOBqFUdgUMASx7AJgI301IsO4YhwFemBzG7bzW3QrfGhOTacjr2HIQFJrecYLf7VR2MzY0tXhyISYaJHytoiyjs%2FvsGHY0iCiKQk4lD9iTtqIPucjAkAouUD%2FQTelWR%2FKp3xi7GaZZjBQH8PZ9VHJM%2BW%2FdqHDeqo75%2FWS1K9VBN4tJRR5%2BcTxfxUEfPvWQHJaX1vsgWNrHMfIIGHwEiVSks7tU0C68GhwAwBgwP1ouH8Y9u1XI0ci1cEAIVxqbArKtguYHUVKgfcWrU3i3AJkYEvdHO1AlFZ8MIPfMj0HAtxciKNGbWXng03mHz%2BmK7Qi7HD4SURXij%2BVSiIA4CaLxZ0RBa1abSlIeiYCWfRZsiQMW8azcv%2B8fSpqhjd5U%2BkQ9I0vujSiJjawF9lEHwGTEl1lHqOHOfJDGHfmtTqSNar2UwkLmeyQY6pgGsaVZ0C%2B8XdwDV%2BF4qNcu1fVwd5YzAl5130NefHEeKCf57CHHo0onNv95vXyFTzBS3ueTCQBwwb%2BVfkQ%2FlBNFury6QXkmD0t5ECbWSrtcM26HJxE5M39PfyhnfMO8z54MdySKkG8sd0jLpAPBRB67oKKNsio4aULO%2BnrYZa489%2BUe%2BguPstyfTNXkQxCGT9LVO9VmTbxhDP3IK%2BGRG7LZQVGeitzG7&X-Amz-Signature=2b660132ac28075ddb49701e96a3d09b80b64f710fb9dbfbb17fc854cb0d69bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


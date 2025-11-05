---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXXZUM5K%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBlgyPOKj1WLJ6SPqJZW3RmSGIwM8TmI%2Bw44F9f2k67gIhAP0cgKet8YZsaiQVkef4XrClbxLnuiqTZaW779c%2BcdmoKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaTDMkILzinDrA6WQq3ANwfVs%2FAeiUGOKah3BODqdM409OqAUoK%2FJLHjmPCavTA06GOgFJ3z5YbexclAWctADRJ9UW9vXUG2hvnVts%2Bscoczue%2BVf9kKKIeAoK4giwXe4XhQ%2BFVvJk91Z1iKxLsmxvB0htOZLFav%2BQi1Ah0ivAtDhN04gE26Isd7JhCPUOMiMy6F3GcIL3xnJJWBsCOoRonZ0vDIk32eOKS9bEOcIkf6p3TGGTugAhp2SbhlhUy6iMyuEApmNG%2BQ%2BVBtCVF%2BF3A8wR8xKJUWz5XnMucJwufxflb%2FHoTcsNclgcsQYNYw0Ea7CJoU2YX7iG7GU%2BneBkJ01s6fZvTBRotgc8KJ144Bl2klGniLlDfKKqjyGv9uQ%2BeRWum7IGRLDirg8LdIWQk4LEfMr%2BuVSKm63RmgcKQXHGdHAoGGMrwV%2BTMV09hRQal4fd6Rzagl1%2FowlaAsQe41uSSQIUAQB2KT%2F%2FuQfyXrqoVgjrGK%2F4TcMH1F4LfIejfgZyFJNh9C7QbDoRtc7amljBjMfC912bn2%2FccObbNRg71KWr0Lr4LiOZD4Bu65STU9HwMbiQ4SKKXWVzcLAYuzGIiL63hNfE8AsB47kHeH1Q0HnuQo9wCPjybDpTh%2Bx8IjtNyMQau%2FdQcTDx%2B63IBjqkAQWvasogboXjLP3hcyI00m3PwpALuKLs6K4xUrPbG0OHUBb7W1ICOygccS6T5hLmR7vvI3HHPRs27LLStMNdD7TEXvo9DMLyDIc2OOE%2B2ZUttnFuLG68hTDpqA4C7LZjPPUylj7OdpaCI2Lt4O38a2nAxgUZ%2B2QvgUNltZO7OFKeAEVJOdgVF4FrGySc22XFK%2BwWuIbpnaJRdmxY%2BGGxkr0YcOIS&X-Amz-Signature=4b7afc38de83f3995f34ce226831b0082a247491720c91ac077e8b54c1c7e88c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


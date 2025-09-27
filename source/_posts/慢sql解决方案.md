---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LQHG2MT%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIB8CWHCGCI69Q%2Bzz0WYAQXHtKaL6yFyZsJCEf%2F8GGemKAiEA%2Bb2booxqFsq%2BERexx0kLEeMI2hoH3IE7wndF9e9uWfUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBwCp2hRnCkI8AEhjCrcA87R6WYyxAKPs6kAAgwndSLq0%2BToP4WQggU5%2BR3wygAFiNoZ24TCHMW1xaChOzkrsil5l4565FNlzdpwKSNQYOxJGE1xd7NLEW7h%2Fb3BdJZp18Eu4y2QTeHNLKMch%2BXx%2FTcoELda3%2FMbu3c%2FvBgw44Ic%2BesczllJ4yXUaGAy97%2F9QOQWO7OMtxJbamrdmaE%2Bi0GoVWBi%2BSSslnNESDCXbdW5pqOBu4TnUWUUiAFdxn41tucuzYuvTNbBUegyT7k4rq9tIk2xHYEiijK0RC3uhqf8mDn%2F%2Fin6nZssM4Zrsj3yUSYokevRBLr0Lrx0cg24Q7IpaL7uzm6mZCdW5YftVXv6uZoWgm5a0Mb8cHwVuMhvTuUWSaWDtoc3NrS5XVzsZJtf3A3WUaP3re%2F53o86AOzQXEfIgli7Ujk91PiQSn07Vpkzyrum6a%2BhphYDvLX0FltVfmDTVwgdLiY5gmp7HW4CxYQ9LM9ev8oFpSFXbeUQl4rAArhRWOtMX9H3xbDR4sGhf4jsAft3ap7T24KlJGRkDTHk4uxIjEoiJaVQdQMu4OOIrr5%2FefGdrsInAUj2B6QwjgimY8C94z7YIzXSWNUDc1k%2BMwURgdSX7nRHrW36hRsiDhuZKXtoZlmVMIPj3sYGOqUBSBg46bNE2TeJOLLA%2B6V8qUe9p7a8YBacjkt3d5u439dfSBAVJN8GDoNHF%2FTcP2XXXKV6NKkkO8G%2FHAf7Un58hY2tvPNFiiEuEy8UgEhqCA%2BiS69Sw2eHwNj%2BDl5kX3A6KGvasDQG%2Bs80MYa03NuhgQZk6uBGySgbt5QP0W187K6yW4JxmEJqt5bGwRAyUzDziLXiE5ASW8heUtIyf0la7lk%2FjEKh&X-Amz-Signature=c5d7a918c1a08c4f023f3ed203772ec68417b640c337ddd788de0bfe2f5c0821&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFYQKDKQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T120055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIAR%2Fw0RBLXrwIcH%2F6X%2FAjCDesPa2A6IS5xjCA%2BhalLZfAiBIUlTe91bzwoJuX0TvuGfHi%2FAFMkmTYMLyWf6d9yh97CqIBAjU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjc6E9I04sIXG4MdTKtwDeqaUOQz6na%2BVK%2FjvNH8DgNEt%2BTiwK9nlGwYqcA5usZh%2BWiuN2CwwBJJP31ba06LvAxEowdooaHxPX8gTo%2F99V63FneNsxQiinaRoopI650kV%2FGL2gaEEWvJfU7DOTNZsdYvCLYbi8muKazsjiHjROHQ6IM2Liw%2BzrYkJ%2BvxKMf3Rn9VZX0TgwXXzhGjLfH19zOVLTUqSjjRdQEFBmP1WMe9IgIlVlnaU3tsUckc3gBFfm%2BkvyYdotrJ%2FcQAWiy7F8l4Bxgp9mHaCraodC6Td%2Fzkx8M6IWuZbSHuC8ndygIq7fykr7n2wKwxX5PfwEtAjR3KQA3KRz2N0EGd3Fqqhkz%2BMVIAEKr9kEbJw16j8w%2FQDd7I6IRPRuxNi4A01njDltQ9CmCTR4qRk%2FmOlGHdvphIprzsTw%2BSjrr4Hn3n9wCJqQa6KMI2%2Bvk9hs5ImwsdHmqkU1Dqe5uKZ9M%2FbR3DhlrAzbdvqpRcOZFbrqvpF4xw3gMDZWNv8XCD2rEqbh1axOwAvDPwTnIQAfZtTlYhuK8srM9Mph56m%2FhEu%2BglMkoIgaAbmWvQl5tpxYPuc3iCW3iKRzqmFGgiWsUrW6K1%2F%2BRGYTwam%2BychKxHBsYIzaaoYUzo6EuIMvQ2uJf4w4em0xgY6pgFuDKMQmYrx22buqaULjdYFSl3spUoHaX73JnM8PLLM92oqurmaZf2HApYs55yKt6MCOBmKWYB%2FhghC38d384SPUZAdGhQVy1OeMtv2TtNONxF3qG%2BDxSjxSbCPRZgRk1htfMOSivWNUgJdjmR7ZIBqhpHp28IBblqR6O%2F9WZk94J8%2FsAClgDxw8TSLnWUHYprdfmPF1EX3aspRcqhOUNOKlP3faSW5&X-Amz-Signature=852e80ab2a84b81874a5fd60e1f330bf880344041fe1222523eebc1184551b04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


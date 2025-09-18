---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666D2JGT7N%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIBvsszpZwN%2FUnOO7GNh6E0kUX4z0lVRcjno6pPntUHeTAiEAxerPwODFDXM%2FwJBH2lxd1rpLlSzNaSEiy7Lz0QTj70IqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGj8hE40QBlA9lEvZSrcA9iBw93jqfjRByiSopqDUX6zrr51ShscgnWRZQB94TSy4NRUcCdkATj1AQoxZAU4SKbVAw3jREYzGc4t7ObGrvQ6gtA84Do3qXwIRqRmOAx44T82mbvkBpy9%2BrCc274L2VriZ%2FKhjGCcqvPW5BrHgaQm%2FsEIThRSoQCsH4CX8hdzV41t%2FW%2BtAl7t70qMS0gMjD1X7FsiPgPlEtefePrUl5f%2FydlAabP7xiQKTvVIafA3ym%2BL1CTnFgzrKEV%2BXk%2BdDgTgsjbIHWIRcs3lCqrSekerVYYjCWhrHP8fHAoG03GRN9c4ovhn%2FBDLkxGqaEbVJtkEW074Fi%2BQgR6NAV6DjUP3LAglJV2WPgGRiGWlAz53XgGo8NOFwCkvPGxsuw%2BukS1IX6gVzcAHHdsOV3ez7dnvbIaVPJJi2GnXsHCqZaIm9NOZVAv6vf0quCGl7miZPcA6rbyrA6OYTUXtT3uu0Anrl9cllM%2FACxQAg32KxGQD%2F4Cd6j7t%2FkjOQzhGhG6KdrR6fsOB6ewE5huNrRis7kHVvznQprT7W3g1c1H1xpqsscNFzAiajxBFQHsP3jF7rYW57T0ZvgSfvTf3hKSYM%2BDaBYSxrQCOLtLiWF7k4WK2zfMqe%2FIAhkmw0TUTMIn2sMYGOqUBbUd%2Bg3OO9X6%2Fa6jEFpqMFeue6XQfOqGjmCzEFghocLWni1gvYnDAeg4%2FOPS1Hqw0A%2FcXGUAlSnIa37NNpbn56xTfqgsAECyg0Tv7USFni8L1%2FDpjzN%2FIMsJYqGc5%2BWt%2BRLJYTfubW9C1HmKv4YvYCsrV9AL1bvUzTlExYeulssRf4r9gkHmIYhPULwKMVzwL%2FOFuZERQZptgbI93jYhed3mljg%2Fk&X-Amz-Signature=fe055936d185af549f4ff5a14e5c2404287f7647d6f9cdfd65ba0c77a1da4627&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


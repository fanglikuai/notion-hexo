---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JQXDEOO%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T020112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIC3dULDok5tjhth5%2BhU5xArbwRBkkT7XF2c9yKD%2BKSAxAiA7ZtLD89cMsjLvG1ujybXytekcUlgaEpbH2%2B4FF%2F4e9iqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO1QDqcrwthBlGAw6KtwDigy5W4Cr6yea5twcCJIS%2B4BH9scSJl65xgTm8V%2FGNF1AH0kiDeGgpuCPPgFbW6XyoFrPy%2BY%2FtW1UzD3JzZ0BfW3pfE%2BXLB0MkFUghxh0x%2Fp2weyyzOOQn4%2FZYA4B1jZN%2B%2BIVznsNojAi%2B8zECOzXNsPGbeWiSM3qtEa0bO3Vai%2FXcpo5Fy3T2g8xPHp7KjWv6F7NshEYPeueYRi%2BxdhSHqmYTFy%2FHOYHr5tpwJyc9sw57qpnglJUgbisOXhqRq%2BUAP7GceET0tNz0UojmJfDHbIOLe7lADM14x0hM1KSy5tA7nqLzt5yrtHQcJ5VCSON0V6pcaeoaSF2touX7w2t0MQZLC1GG4dxa3X3wYoz34JlNclfZMfYjn2egxZBwLPXjPX18Frnhz8GpsRrhmWvsDqViXpkwk7u%2BNFhTup9WP%2Bi1kmqMo%2FuopEbzyqMwkB4m5qK6e6DFTKiv4CP5FBD9xsrDLUOdP3VCbKGs3AP7%2FhD%2FGNl3RLQFcT1jOdRZ5mjlu7G5VPfrpiugN9HqSPenOYDuVjY2O6QqVoYU1hjYqiwQczbwBp0cFA8TkTQUOYKAqhuDQywesw5eK8iHAzdPKNfFiAYM8ih%2BqUPh%2BQS24r798XitcDK%2Ff%2FUy5kwxZitxgY6pgGirZtOL4igrEU0gfqMGddwYXgD8N1ByrHVtdFo0rks22vVcW6MEk5rrPuL1csAZBtvQAWlpGqTJyIwMfy1tbmkjqBcGlDSEt37x%2B3FRlYJbX9%2BVo4hMS1q6RidaEhdhhK7ON1nrW2Y6Y8D%2BCDQpVcaqj54oza2NQ4sueEUHAyxyiPnMj0CvEsbu1XUMhYw7TOJJ6d1Ebb55fxJDOu0WgQpEhHHoGXL&X-Amz-Signature=fec3de365f158ee880de265bf5bac58a09a28330464bdc58efccd322a00ecd86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


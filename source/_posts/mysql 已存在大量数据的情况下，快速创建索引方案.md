---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663X4RO5M7%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJIMEYCIQCWTY0spzbyCjvTmHC5eXOXrpImZ%2FlQ9Sa9kbD%2FTnNmDAIhANtaj%2Fjr1%2FQG0zWSrfhwktgwCJQakKmSW5ocrbCmFLSnKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy6qDmfLIeFvFho1Vwq3ANhTlxuorgvrwjBJ6VQXH5xUx4wZLWiA8AN6AID%2FrCDrVGD76KYZs5vXMvMzuhtxsNqpLux%2Bf8pgs9Bvnx51KxwCqyQoWo6B2DV%2F%2BVXlvPxGmIgVkf1K2QImkwBuuDlRjxgL7037GFJ87ZVyOi1bgwzH%2Fj72k4omtT3kg2CA2Dd7FL65OIWp6hzuBlyNKS%2Bzuvgv4jGKODu%2BQ8rZPMfBSEtclABvn40oGv5oNzfyoSXOQKzeTM6XxKGQbHKxYy8%2F%2BJ%2Fy%2BXTwgfnpj48vDt9k15m4jrnxegOLC6I%2FvwXg0zB%2FXKuMeVQAlAOWP%2FA4Z6zuOeUS%2FvIAGuU48PbzIZbTgV9WgSF1EqJnZtGkpbKi974TOFqrsj16IW4FEi4u5SU2k%2FQcGbiZL%2BnjM8t3BT0T5CF4Cs57AHNT30xX%2Bu%2F9iTykCmPLJRndHyZxQvD0kJ2Rk9dtzEFH6COINsB8WNNlR5HQegSueDB4HDNJ3gpItxpuh%2BgZuvf3s6Z6pWAUD6QVjCd5DreiLmwarhZEvWeN9l28d8uN20zT%2Bwjrz5samRwVGSFFYghcrJDZMVzoAPyYvYJAa02ycrgdRcOLZQWfYHlFFFFYIm1X9INA%2FIkadlDIxX%2Fdm9cCCL1sApuvjDggeDGBjqkARIxhl2tsOxiMavUFVnOBipoVRJLrxY3kEc1V4PW%2Fq5vynwhkTc2yx9SHeIqQxdXZlOwyVKEk4Ls8734C0D9ke%2FqrHGo4M9Qe9qDYouKoWk7sMH1SToMvTP%2Fapzi2j8J0yhHqWVqorQd8sXNfzaKhSRqGWqUnDXpfWvpPFHaTLGHPuMRfmXYZIWeGXaAIiEEpWsYQnznZ0i6pEwDAsDMVhJRWcB4&X-Amz-Signature=b495e952b151dd66a4528bd1d44c46537d31a8cbef4b154bd208a8a25ec23d6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULLCZIET%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQD9PxMAcfILWRCOGHXfagzqSitYSftvq4jYRsOEQuh3fwIhAOHit2RpPUCpvR69skE%2Ffm2C%2FWRM1%2FxN0WB65vW26zwsKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwwQeERlCaD9u1wDNgq3ANqQrV6punJBHuVJFnqCdRnnxzDnFjnEbQjM906jtBTMJlwrxjSznn0Fa2c3qTyDS0Ad1%2BmaMB%2Fnsco9Rrco3dWfUymwRCXDnL5UNPzyk5Mk2uzcHvHLt1wopBbL0Nw3mi7aaTx6k6nYT6NvQ2imfUfb%2FCSOyA7AbE2pK7%2BU0Iuhdc%2FLA%2B1BlIEZ3S67q5zqJRkXCNUQ%2Fv7kVDw2V%2BnSFhTDl9kEkX50Y9dg8zEuMDtz6cAhrvQ0ZrqE6Lk4S1JN4eCVB8eXLtgtea%2FlHzb6gSOOIOWZsFoT9PFuGjbqO6AtyaEqxQZ7hrE73A2oewHYROzLWk4R03ydtuEUa1sHbJOAom2eO3Rfe%2FGo6gJevmpvOcw0EZBEorFahpPioCh%2FBYaM1zwx8qOIyeIdAjQBqUx0geq7TRb%2BT06ZiBUOrQQfqUyqDkNTn66rKaRUSEhTo%2FcGo%2BQyqAOvHcTUgs%2FOssHqqGLTqKRzkoSMfEUKSkB9vSIiVbjs49UlF%2Bcyn%2FC3FoSN1RWKQPVDCcZFxuNDTFxwrJSeLeXkFNJrNH2DgMPbNKBgBnuIq%2FKjX%2B86OBa87E4OYbjmHVHq0isFFzSnBTBzAcAyEA9gjC9Rsv4DqG%2F4%2FmB5buPAehMljvsdjCt0LzGBjqkAZTo45BCzYRZQv%2FJy3tj7DGxQLjD0rowUMy5QVsMvdYqxN8rr5%2FpOWy1qME7e5Qp04xoTAogJJ3a1sQsbDW2IwYEtNRrl9jfjMy6RoIGn9QuUNr1vh2GfoFRu8g5rJax813ZWTZYQBLBMorNvssWzCcRi9SKoRtm%2F%2FCd6sD9RBiV6IxvvVsrfUCSzyh6SADtRY1R340aJClpgpcfOzbA3OuubKUC&X-Amz-Signature=795c27122096a87534832bfc49fcc018183f6ae603db983a9fb4c59b74f9dc13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


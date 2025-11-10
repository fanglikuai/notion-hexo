---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBZXKWVM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDon5dhACEcJyvCIIZUNJGMZohVqOC%2FUlQ0YTHC84Wf1QIgZc%2FxT0Uxf7ZLM1w9EtN3q7CF%2FShSy7oXA8ABxOo08agq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDHqaHsID5EEl8%2B4hxircA3ZJHQ5QVox7maAIKNG%2BxIZqK18NbCwVVxdUvzwpvK6q%2FCLHFbBn6z2llAelddFoN6rLk47YEQGitU1GfMHyxIrnHrHnNAnVqo2SfbFm%2F1O614pgT5g%2FoqwGjSkt2U2drAUWOSAB%2BxeFAQhKCZ%2FU%2F8eUvQyxzM%2Bk5nKqaYiixOZ0ystuqBBivkNZqB698pJM436U0mrIn9OGxhyLQCSvcK4GCT9VZJHEAGbNAKC5SrEEDJfjxYd%2BntTCrasPbCQh7O3oa1B9jPcaxVTsVDB0pmutoTkm2kGJ45mSJkOfNdrhBBN8ZshA8A40vFngpm8woViJpZORUfS2InZ%2FVA6T8ITu1umuhmOKJNukUKL4y1yXmGrlz%2FBFWr9c5iNqgkT5s0DPaytxsH%2ByvTPkzIT%2BApE2vTDebntFL%2Bbr8uUgt7M1qrrz284IqMFTNEelHJG2dFfSbrGKBDKE6UbHq%2F%2BX9XHWSdZthrK3DGYwe8vIeq7YT7nXURU%2BWBRe%2FL5NF9bdbmS%2BR%2FDauFFFtY5WEEDJC86QMO0WDDR8xUbAeiiFGr2NBW2j3PEf5red%2FJb3N1NpmoRNkpYKheaLkgOtXmlKdhqaQe0i5%2FaGETyz0Ah5bYg%2Fft9Zykmnj8SRd1xbMMbVxsgGOqUBxskOxoO8xK7KK8139zhWRrQTsswxmJDfYJjFCwzc0oz60Il1YPI8Dv6ZCa8H9l8xmyyrvN%2FNCT2nprQ3m32wbj7WXP5XeaNh2DSn%2Bjn6ISjbjHlYSETNXSoK0awIGnabgC4Nt3ADzUK%2FV3J7zYFhX17RlZ4oS3Yrt71fwD9kdvvk57NXlTQ098%2FucL%2BRYl5vFnSbK2x%2BREMiZXQFnFWpLj%2BoyB5y&X-Amz-Signature=249a2bbbe6a7faf40c5e8497ba224e005b5c1a79bdd67d304546cf38a82945d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPGDM3AM%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIDyPoZOcni%2FjoCtyoSi6ijPCJcWKmb9cFptYJg3AOcZ6AiB%2FC6HV1ANl64Z7UZ%2FaEfpL1CKzoC%2FYg7rIAl4GBocMeSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcDFdZNDUIF3bveb3KtwDi%2BwKzsgQSKCTb6pSbynevqJ1H%2Bk7P5irY5baQTddaxGIvF05lmkWKrKBUm34%2FFwR5789huHZ04SkyHdUofZUUnWf0H64BvTURYbLoA1i1KBSjhGmB0YxPbHY23AoJsZ8PbjwUhClZMrHb68GRzgAU7hQMpfD5rTU0g6gPH%2FXEQrdMdcau8Az%2B%2FpsHHRbuuAjNYvo7RXzAk%2FlTFlVwmZijoMTfg0r3jIh5wO%2FOnIcWFOVl84K8LzfprwKfaG%2BVdnN6PU25ZTYjBl6vgvFNOGB39XtMYtgKX7dI5Rq2ifsplM8UbAT7m1iO3czcSzaV0U95EppvwLR7OCYGiXOSmJDcrUnxN07xsrr1BsESFiPNYsmgSpFq13PzwCCPKQaOD2VLdBxcp8YJiU2ke38Uk7LOcmP%2FWK7fn3Vg8zBMdJ33gbidn1kgQhfun6zNOaNXDbZYwdRbsYOv2P4LGmYk5FIBzn0hPDbR%2BaikGgUonbEGOKlCm9N2J5SFZZOnLGTGQSMO2DM70wQKosFB6ZCQOQAoSCLuBP2n9%2FdMZeH6JHhuY913icZjpkumxrlwSlaZOM5oC1Ay%2BYLAbcrxQnKddlnIzXw7ZxzwBXLM1a5eZrLoLFCixNiiaY8S2eYB5oww9WRxwY6pgHftiEz2tUOuAD97rrDxuz5UPOyUF3W83f9GeAgVmaFm3wqQtGt73ielpPH%2F5VXEhUb1psC7RatAiSyyMHka4eVjDReko4NnIAgs14znDrXlYcvGwWbYgqMWXIMHEHwlJnwum310mkH1KcR5j2Jdo%2FDZedca3S%2FoKB81jevS%2FhCazUZt7uUv6I%2FH%2Bq6kDVr7nOnSA0dMu8sQ%2F0LauvMT%2B85l6r5J33c&X-Amz-Signature=c327fe474bddb8f3467795d9d86bb85023dad591e6ebca395ac0de2a70345f2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


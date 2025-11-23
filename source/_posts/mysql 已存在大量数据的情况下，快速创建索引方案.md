---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2NDRGDW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJGMEQCIFWICmlE7rvwrX2fYz7oBzwvKX%2BxiS5wf1n%2FNoknEUfjAiBQPr4VYhmCLPVvRWHD7WbGroNWYNM%2B8XaP7sMa9N%2BI0yr%2FAwhAEAAaDDYzNzQyMzE4MzgwNSIMpNUYlrpF7EflTXpfKtwDpfRYeNfnmlzvdnEhma1KJTxeIbXgWUGUmf92eyYMHhCYlpiFfUYbHuGeGojG5IqN2Vgp3krZ7jHpJ%2FKje22QyZpv8pDVr5GKl5yCK6fheCkGKtBGeUWaNxSwQ76B8EV2SoMfeeNcpXPejaFUMdMjLjijMik3WHCKzWpU0Mqzevpwyky9NaCirUZFBTPaohwl8jvBVwE%2F4kCiuGPzDdxxOOQpgK5jRrT3%2FV5T%2FXFZXujNXjWOUKGzk0arF%2BK6VWqVno43oBZuT9j0CPPUwJlcxxXvEbgc6P%2BlMPLopYBQMR1ozmoQ6T0E8%2BiDK%2BQghs0c1gggyPKsYy0RhPGLBsb51HMns4KA6HoDuDlJER2dgn56iLR7Wde9Bxp5peCLtaj%2F9bUWmHGYLUe2ng2pP8AuAHp8ZzZP5OVKvqrHYh%2BeQO79LYO9DJ66ZU83inT%2BzG%2BixIsY5TZ3tgv6f9e9nr7x0KHSX8FXAswc%2BUFXC4Gm%2FgpYPtOSGxRkxRlo%2BznrFJWAFv5CksXK4fzhZrgqEGtGA69mik6ZG2q%2BCZSTxQdhNiiI4yTemjjv4KDHzMhmli%2BudaafzuecZJCYipURC3aIwzXSoi1bWRy%2FDWOIrJo%2Fa01GPyHgcBqMto97QXgw%2FLqMyQY6pgG%2BEZuC9kSxkRXaQxoP1lhGk%2BcRBWdR8CWb%2B9W3ID7HMfHUhigOpvM4%2BJlk%2BR1DE6%2FC3zZtWkUex3cDOqZKOI%2FCiYU5MRNeQWPhJlcIrTdJZfa3hx1Za5ZlMF%2FWjSdzqAyqdSa12NHswYzc9HPiwlzGKrGH4qemp9eZHwBikFf5I2S2ORlhsxAQtsrMkCKyUtoH5iXd2LcEfuH1jpQyLi%2BKYe62fzF0&X-Amz-Signature=6d0c4421d3f93dca87d9434907296670d5231dba4864fc839891bf55074a7452&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


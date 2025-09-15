---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSV6MYGC%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T085604Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDHEwDi%2BJbHCY8FhpsvcwCfrKkQWVGoDWOJOh9nPRse6AiEAsrNQkx4YBKlz1LDmlCJGtFr27WjpqB%2FfgOJ6vOd66Nwq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDNlPp9SpLmXHHjoVSyrcAxdqTdVruFbsjRIED8CdHw%2FqTEDxJo7sutGDnEY2Yi3wwvqYelQlGcrOSHg1%2FNCzHgYOl%2FFF8TyazkMjoVaCm4xDXJIqgeicM3hxxw6LQh%2BQH%2F9Qx40Q7%2BVw%2Bs4qdOmrF%2BrKM3%2BI4HW09ROlW8kkve6oy4g4ud%2BDnG8tqxbeReBeICGo984n7hFKH4A3tJJZUFgF3kefh4UgZRMhK1ts%2BAfsHDQ5q6c%2FOPuSjhQf7ySNdb3J9jEL7BCJvGYmkwMvX0vedHZsUVqznk4snfMvhjOU24pAgo0PXZhLGNANgAFsD9VedFzjrU2e0thZFIYoZx4NSohoD%2BrQXVc17xmycd0t16yOCwjb9UQObp6pm15nd48cZq8sgJDsqfYMFXDam6K1bPNQBfvDo%2BWPaTMnsXN9Z4Cu4JoInYMQxM82eAo6SOo5G%2BAE4vu%2BmLFIasqUnf4pJDmvjaiPVvTIWbQfrbr1WQYB69YWtRgdWbmPwf79OClZW8AmP9050a7tpoM%2BF%2FwwM984Y130Sdi4etMuIm69g0QOFiPQvhXihPNnMST0gp0G%2Fl%2FMyGAKwxod8%2B5Yp0NCkzCkkvVzXgyk1%2FnEQ7jsYEtkpWohczyPXwZqn%2BzICmFxW4JsWpCh0rcvMLWTn8YGOqUBpdavTBxjNR%2FohKL6fgbsGGlHilllFXyA35BgP9ce7Jrip6SE69hEb5D1EfVkiJ0Lih8pATFR1X8Rd02XHN%2FKEs78usgh4nQQJIsLVQVECnmJTLqJDewtPAQ8193UFbQt06YIlxos0hPNGY01NkMyyByoQY3QiFGWoPjLKLA3oXGhncDQy6s962vKXnatXkiL7VN0nCPvDznkUF1xvETPAfZ43AV6&X-Amz-Signature=6da561e988c145aa14352ec4c1f3856abf6aa6c5a44e6e8720b428377b33656a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
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


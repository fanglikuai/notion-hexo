---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWBR7P7F%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T080059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQC4PJjKl8DIRLjVGzAbgwYu2Wfs%2F0Jel67huYj7i%2FOAvAIhAKgGSry9aERvGcpTx7YUQRGxl1KK%2B%2BF3C6To2VrpxRE4KogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZpnw7uqgAJU3miWcq3APAW69J8Ld5dU1sCs8JPjoWdBdKe1qw%2BFRGVoLrLUY6MZrUtYx6s2YovwZvTo4Cugew6HSQNDF%2BRnSdfSCFOGaLsWdLdxf8AXHB6sjeulxEdEqPIJaqlTtilI8IBlDJqjtGeYVhtc589ojk3zqnWXesON3SR9xgdDAcmo22M%2FO8gNbTDgVmHQ9vLu1JDRDOQC3NnOwadoUzeKnV6C9sClyMN2Jx7TJrHxLXiM2GhzwjfYwqr0Jt5zJxHipG5TSvojquvTB0GJT0lzugpCmlMWThhE7cWrP8YkICY3Q5%2BnFteUn8mC0PGTJLqbKLDg73GbmmPAFCZAkc4InEBIWJcaR7HQUhAEbx8KSh2oJwunAhsQu0%2BqCzwpk%2F9qf2dVeltZQnjMJmq7z0wjElpgHngRai5LLdPA7IqUvc6geO3JejgISyEl2pYJ4mEw6Y3%2BX6KWVc%2FdlRnGILWuXpz1w2Vs5QutalbbGGs9N8bsmoxj%2F3MbYuGfHLnVonHfWEkMEFlZaxN7g701AMLqMVRu0Aq3GuBw8%2FhOvn4ADv1JCAuUvIGEeqeFLXvYdVoOlOBAYIxPeMansHoc8GwVYOCNiBZRorTRNv8ukFxxAhad8nXzBxaiqoyKzc8rkwUEWFWDCsjZjHBjqkAV%2BFHWuN9xM43yX%2FyFScn9ddAMh8IltVi%2Fti1AtYi%2Bq7I1Atqu5lYrNZogJ5OjiW9%2FM%2BLf923neVKpWtnmyAY0rqT1AeNvfXxNZokQPl9dJACLDzxw01utT4rNcKPnkfmM5ODsj2zRUVPRBSgOvLAFowWdanX8dJhq4TM1G%2BGGQ8sFkRsTzDY3PBBtCn6CiawFIDDcvlnwn7yx69ZYuTcdNv7bvU&X-Amz-Signature=f38a9291731d35d172a9e5661779c640abbd8ea86edf1d721896cfebb6b910c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


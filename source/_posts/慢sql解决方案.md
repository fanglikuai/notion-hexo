---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZDJFYD5%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqDXs3Ujis4sXtAD26h9gWdIzsfvU1laU%2FiDDVWTHakAiEA3RyXafnGVnPUurrwzDsoQ7vcHFWKILbvE2Xgu5WyKkoqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKmL9JFGjEOyp7TwjSrcAwJ6whGZ5Nb2Q%2BjxNvF3SH0tXaDPG6oK%2BepvJOY%2FWpL3KSqTBYspiG8bjgFmhEhI74hbT2AdZn3SiFIAPe4bLbuGeWkNLR8Hu1OIhGGrjNjEo5ZM%2FmTqyrvhC9lQvrM9%2BpW8hbEW23LVWF%2Ff1VhSyj5lpSHgZP2Bjm3IZW6rptgxPQHxx1LqntkR56TS7rvlNRjjQlwxuKd8d1IL6LouX5Gz%2BO4dJzNU%2BKc8bO%2FORBB3zOQq59ma9tUJH4ir9xHxUmUxMtZt6Kp64m8p7a5%2FF9U0PAi08KuUOXTEy9blyy6ciSePPIi14nabB%2F6HeEyWYgcDn9%2FLllRJopxNEvBGRJDHB9uZoyKQK7LPHTGl7SEvtX%2BcgGOQsn0Kx5f0EYVHrD%2FORNYzE%2Bna%2BryW3x292SA0rUCO%2BmWIfgVUcLPuJMtT6nwIpyO02rA9elD4AMHe8wPSCSCxb7PEQm7KkNvelF9IHTkXraNdKC17ZwaqapfkkIRpLWhRm8OgM1FOjugsMKZCYAi7Djl1E0INFDDlaQAWgCULpS0rUR2Mkf5es2RCrsQG%2Brimu4Ph5XEHetUHpUCxQhpgridzWakm%2FDrj2q72U%2FU2MDCDN74uCFQG%2F0s1LS0tOQqNJbV15c6oMJbngMgGOqUBx7dUhBEdO%2F3Cg7RmJw08APL4B8sb9hYOTgIx%2FcN3C3mdYL3qlHq3%2FhH7cgy2pi07Z5eAcSqiUUdsRp0q7pgIJ6JC1GZHRiEt5Yy9GV5uk0NTAqHdusLMkYs5%2F4FlMzTZepo0bEC%2BdFGZDwhNY4yvgaVMWEAF4ZK7mGx3r4f%2FovWOd7KThvzchcY1RgvWHmT4AVU0COXJDX6Ne7Isz3aoCimkPkuF&X-Amz-Signature=639a85d1ddffa01e408118c11d743aae0b8147734a98a1a0ab53d25b0c540d44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


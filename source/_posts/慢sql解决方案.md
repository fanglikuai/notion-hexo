---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTLAAY3L%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC0CKMGOy2oCdC165xfiXfkBwkrQHoVBPbSpY%2B%2B%2BO%2FmhAIgSHOVhDZ9BcHcpbB6CidwKcVbyohCcCsAwbeQ%2BOHc1%2F0q%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDGEgaYZl%2FVK%2BDD1YySrcAwzj7CGFhO5WK2pmUmi1Jla646MFegnGZnSA%2Fywoh%2F6mvbWpuJwh2QGM6cMk107YL3GCSKVjDTTtBcD2GNvbQ6NtPb4Hh%2F9SKA1t9aTlN9W7ileGuaNLBRS%2FwgdVi%2FqjIwTglpclWUJIieXoTmyuUyO1nmig2RtD6a7Og2uHnydG3Ddw4Q2iDI3xhNba4qyWD8QWQlkjNsjOvM%2BhKqUZI%2Fml3kpPYLJVjqdZcynJGkb69i6fCxjSsIPnSBNrIqt6YZFacWM0ScE2QNeYoxKzYX%2F47ghitVcJ8FD%2Bq7Gb43mKSsCuy4zHrU49IYxffLpYPXeH0xxP1Yn5JaR3k%2Fabdw1SPd1yIaWoaE0sZFLUbJrpjwI5PdapbTo3ExpcbR9iPlQghiOy7rt1RoWvQhN2doaU22pWKmgJrMEEKlvl29lPJ%2FF6W%2BKEc49%2BONuzP%2BOPKFixp8qkpOCyJ20Lq4fE5up3a3qz0iowNapjoySExrBdDZytSeEYd6VT9PPWF9yA7SS5uPGuHm5fekH%2BR3TD2nRIbKuZZF6HcZgzU4bklVweaw8sKAUmf2uS0Gz6mALl08Jlx9Ijdf8IqF9xXGtN1yj7q%2B%2BEbkfp%2BFLXkMvON42Fzl070DEUa9fZROZMMJnagMcGOqUBAgU4lqX5ZUDvPTponGdnX2LHXThaYt%2BVMJN2ePos1LvCyfmwqd4eUW69y6oMQEfrBUnL3pnbIyKIiW4rFlS4Szi2HSuItlJtmnbs4FgCSehlH%2F3%2FSM6xkhi6D5iRXg%2F3uPHD1OmoNL1hWMPc0IxK5VlIBEha0OvOAfEUEzsni5Krwk50zj%2BZPpEseQfsXLuJIdhy5nECSbT62dQDq87QTRBf7K9o&X-Amz-Signature=a0da82b4169255bd76044a77fef630a7d25ef76cd05f11f7ea1b0439b141e7be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


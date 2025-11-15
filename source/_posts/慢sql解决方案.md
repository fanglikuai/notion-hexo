---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632E5UWOQ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC8u1pi4EXMJVu%2FpCqbfUkh%2BeZSKcj5qZ%2BGc5CzlYVh4AiBzmpayOdKQP2yGjgfVGLWAgPyKpwHnVuZOXoB4BsasXSr%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMUA4M2GKqvp3JhXCZKtwDtZmuZZX%2BjZJyezPKCDcNjtPe%2Fb2dfDusbm5AUpHokfUMDDVwmFzX0nAfbAGy4s36B%2B1AD899ltbK1905B8CXyNEyvIgT0wqsjYuqLWvDpwozOrNAJ%2FxASwyU891dS3P2ZoERPeH2OllmWiF63dJOXibxLneu3YGFRT3ZFx6QaEnX89eE1T9PIU9AMBuoHX5g0gFnYW4bz6n0Ogi1izQrsIc3%2FeE5wi7CNXspTQww0pf%2Fgwny0%2FdQ743nqbeSBh8NfM96Lp9NaHy4JF6%2Bw2F2kwY0mrylrCacWi9hLS1F1HPwOgfk2oQy2%2BLeKgFKSfwxaaLi%2Fpls6ihFvk4TvhmdfDTIM6cEONHUUdX4iCHuVxrIQbND8Z6YookZrTntr3EU9v3eQ%2FHZwhZjKANlNz3jc9Nd%2FFE4FuNPg3T0BW7bsKQGkaNJZ9dPXC39p2kzMWNqj4oMSp%2FV8u%2F9jz%2Fl8yLLaQB2L%2BeYsssxE2y9zlY7h%2Bnj1JT7UpROvVfwq543lFkfLz5e0hYlxkseKl0V%2F0t%2FjV%2Buw0etHJRF8Z7lhvqOP%2FiO0fUHQodGFsh6goGfXMLEGA6rqp2paJRjWxchakzDn%2BdHZtnpguIw2hDT4cqbTieC0ta91FT6sUuxehww5%2BXgyAY6pgFhaPIFCFwdQVaMoldCOGeoYvq8EggfWAczzL9%2FaX5IwfqH1XxGCiDYgDUj%2FIgH%2BHA%2FwSV%2Fceht29UKYZaRIcwEOHHoBfCuRn3WWs1ZhAUfRv%2B8aBrLPEOFo4eu0e5JLSRP3h8JdJXmvNuyahSh6mdui3QFGbrKEr2oafJzr%2FYo5HujL0zXcK5K6SintQLferPtW5PY22szI3QmNt869bcXU1zXznqp&X-Amz-Signature=d12106eea0e0a5e86f654f0ff42c7e809a8750a0d6f6ee2f351f9bdf0710cf09&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


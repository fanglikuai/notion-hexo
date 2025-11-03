---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GSD7VPU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCOw%2FYK4akTUfv%2BwsBX4VIASYXY%2Bc3yJgKts6xp7kLFcAIhAO5ypZLWvuxqXtND0bmDoAKRJG9enLSnlz9zE3MZnTaoKv8DCFIQABoMNjM3NDIzMTgzODA1Igy11nQH0UUv%2Bs%2FJMIAq3APs3BU4x%2FoJSbCJK2xA5Sa%2Fd2xWZCkOFPgXsb36XjfBQ7bIrfzsacZpaW4B7xOuOIncEQ7mNDFEI4l96ZVjW%2FG%2F5zNp3YZSRi51VxxuiJ2slt%2FG97kJAo%2FapboLKayEiYzjufK8Q10DYzrny2LSAZ%2Bz7IRC7DgDtun3DsalHadQTcEg2gqVnnMzNZ7drkTKI5THpMuAz058Nr8bAZLUD1mvWZ7ElkD7AQitP%2FB62kl3qw0uwZevTHvelp%2B%2FUIAhqyRQyVdpg%2FS0QAthxn8Qcew4pIbECZioBJLISBoPx9HxsgfW5vfkwjdxrLGULcMCVEEpB3rcvGczm5scjlqM1CQQnJ%2BC7gDQW7bFkJ%2FzLyHNjlmbf9RC1t1TugYaCoDPiF1vrWMEez5gz64ZfBUBSaf%2Br9uuzft0nfOBhIbk33Rp40cHsiHphMldSKBAtzvWEwiN2GEOrwbPrZ5N6xm3c7ghJ8%2FbHWfbmyIS2nemY8aN6W9uGdDjR0WXclcGcUUO42u1sQkCf%2BsJt2qM2khSHqoxM8%2Br%2Fu6H0nOv%2F%2F6PryyQxGLyX1%2BDNwnKj0kih9%2FHvnq33mnT68aU3Hj%2FYjiYENb3NOVQx%2FbelD2kw3SokB%2BJU3an2p1i2n0vmpZ91TDL85%2FIBjqkAV5jQvXQ22vSoEsNYrecZf%2FBdB8CTsW0OhC934CZtUe9hrpehZiWiaE4ECOYHIZzn%2B0qb2xV8XD7w2S8n4aVsCx4nSLQA1F2wK89LiS9a%2BLdegvGRSDvV%2BBqD%2FSmy6lWVFrWnqgdOQN54mNuo0V9dOzP3pl0SwRIvk79ctYV4%2FcDFthE07DB6ag3vQ%2B65IDKVQGm4pekiWMKIeJNKPic44wBbN8z&X-Amz-Signature=810263d8200dba7663a9da70bf815d79310a6d481b87c6800d9e81d0fbe9a76c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


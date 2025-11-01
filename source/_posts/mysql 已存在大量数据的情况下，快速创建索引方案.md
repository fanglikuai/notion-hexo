---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BFEQ2PQ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIB37WkeYaHPBPUhWQnBxTDMdwA%2BSoxO1w6dxaU4LQ3vAAiEA9%2BrPWNFNbVTLNjZtbQUv6xtxsmVQwSVMrcVgYIe%2FLskq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDEUrnEFEcWozRHEqgSrcAwdzKYAKgbR5wl2smgzf5FguRrj%2BlATXO3f8fak4aztmk0eVONgZiJgC2Ux7MzfFd%2FpMX%2BYEFp%2BtH9OJyU8IiTX%2BTmJIkkhh4hARwU%2F36tEs2drPNaqyAPqwGvAf3f2W%2B9yg0LKLuQYlDTJEPigW6hagRV%2B0JzMFLPk24T5h5cNx%2FAwg2yqKTcCNRw6XY7KjpwCIv7xAxLgpSEDhaYqUPDeVT0%2F8Uotn%2BeOupgT%2BMj%2FbPlDtB%2BUqS6iKigHg07vUD%2BHlAYWrCAvjMJpdCPl%2BY9UFE8UBQ4HQOSCUsfg3bHdO6P4St3dhwMdCMGvh1Fnz3i0jcAZtmTlpOJyAbAVccNP5bOJGAIMJsqzn74zTktPWMUwA2gdc3E0mI7nK5QCM%2BNm2YfT1etYTawYiUa52SsQfXvr5J%2BDIEVcTeDWu5SHoB78xSbm4ZI922s0oiFOoYLkq%2B5Omh%2FlWtQLPXO5%2B4eJbmkz0595XHAu5BPAkwrx%2BDcZ%2BihrFwsw%2BHpiiGAZuv6xnyJmDdQ49RK4tTlMllMYKU7S8mbuu8BPTWLMqzwGCiOMHYHCiYFbwWgQXafmWXA9OQI%2Frlz53TVq7DCjA%2B7Wy%2FQ57xLvDtEoVjWwo1PRCtiDCJGjws6JS6DurMJ3UlsgGOqUBTBszKC64kBL%2BgLWyR%2BoUtu%2Fm%2BxEZkUAeuXPNEfLsYZ8JysfyNOgXGQyPvwm0AcOMGSXydFl0MxuCNsCUCjQRgJ2FEx5REFq9oe%2B%2FPBWMGhLFuBJ3nwpDPxGrY65tXeqpH6fsI3jBBjK%2BfjtAlNUhbgynpxtcvFLi7uTXd1Bv5MvmJ5T6VqKd%2F2uhBZVAc0FX%2FK5DYGN0Bef6nxxLDGCLMQDyT8%2F%2B&X-Amz-Signature=4de2e114b547fd04c56db54f69b4d97b80878a8b68b15772220eec427f2815b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


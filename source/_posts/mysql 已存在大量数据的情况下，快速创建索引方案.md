---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VICT5K44%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQDkIZH%2FgPl%2BbCBdwzCZs972rZ%2FCIgP4AvvFyRnv1m%2FpCAIgInLM%2FEFoa9sihuqeKoyZoPIHbC5zkoutuxYPG07oztQqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMzJ6XqAmktIAHohRyrcA70Ai8M1vo07JbutNSzZwMnoEZzH3Ev8tY%2FmB9wfzFNz1Lo7WO1%2BVM9VQIzfleIllhhZ97DxkWde6ZgxJBd3EDHLomLLiSy3HWI%2FaPwbRTIIXQiJ1nj55YM%2BUJhJU4pOOi9%2BVWnMTGwGvWqMpsj7M%2BqLUqlSdRx4GTT7OmubHYJ4kSUevFSM48Ply%2B6NsxZtubqkDYTKmm4cQFQYde%2Fxtp6uBROiJrECTCKfbZ8nRnxGsmQMXSjtqgCBcay06mY7UO%2FSm%2BbWPWuWDOp25%2BkUH52o9ksq%2BSq7%2BYoZ2MzdA9qnA6GMUxp2YE6Atn3%2BnDzgHtu7FuyAvN5L1qgrwPdqy2wUev1H9oVF%2BMpW38fa8E5Py1bJdNKkaH6dkfmO%2Fz0cImwhtb3NZeOlt63LN0SNupopcQwlraWwvkYZhSFXzVNtF7RMalzHFxqTAPBuSfvAv0bnJxqMLxRA9pXRPswPRQHJIa5LVyW0SWFdOi3rG301nfOFMxC7nb7vcawcXiBZDb12ZWKZdhS9Gioah0%2Fkg04Ox5r2yxtTL%2BI6Y6CwWWrRnNAS06B1rRVe%2B34dSGFUz8kzCeBIUg%2BrSOf9rBATOJDtQU5bXvJZmquhTRjN73MqjGMpqdxzvKlnTtf3MOLClccGOqUB79c%2FLPUPBIkPCR7M2a%2BGhTYGTz5bVPSoZMfQyr6QqSRoRvzJCf4J53BfjylpG0xnVT8gglJMxg5%2BBlP2%2B%2BraUXKZE8HdSbz%2FWNtI%2Bs%2FcbSiZjHcStdQzb5QMlQicPuvo3IXieT1po2ODkRMcJiGuF%2Bc08u2JVWjvGlSlM1iq0Qrs1XiZ3S3v9B8qWTCNyMJtq5%2B1z4odHvUNogoKHc6TW%2B%2BbY5nV&X-Amz-Signature=c717ae0acd95790c67e67264a7d36ecbcf35be5744f24cdd9d821c98e7b64297&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


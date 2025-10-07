---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VICT5K44%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQDkIZH%2FgPl%2BbCBdwzCZs972rZ%2FCIgP4AvvFyRnv1m%2FpCAIgInLM%2FEFoa9sihuqeKoyZoPIHbC5zkoutuxYPG07oztQqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMzJ6XqAmktIAHohRyrcA70Ai8M1vo07JbutNSzZwMnoEZzH3Ev8tY%2FmB9wfzFNz1Lo7WO1%2BVM9VQIzfleIllhhZ97DxkWde6ZgxJBd3EDHLomLLiSy3HWI%2FaPwbRTIIXQiJ1nj55YM%2BUJhJU4pOOi9%2BVWnMTGwGvWqMpsj7M%2BqLUqlSdRx4GTT7OmubHYJ4kSUevFSM48Ply%2B6NsxZtubqkDYTKmm4cQFQYde%2Fxtp6uBROiJrECTCKfbZ8nRnxGsmQMXSjtqgCBcay06mY7UO%2FSm%2BbWPWuWDOp25%2BkUH52o9ksq%2BSq7%2BYoZ2MzdA9qnA6GMUxp2YE6Atn3%2BnDzgHtu7FuyAvN5L1qgrwPdqy2wUev1H9oVF%2BMpW38fa8E5Py1bJdNKkaH6dkfmO%2Fz0cImwhtb3NZeOlt63LN0SNupopcQwlraWwvkYZhSFXzVNtF7RMalzHFxqTAPBuSfvAv0bnJxqMLxRA9pXRPswPRQHJIa5LVyW0SWFdOi3rG301nfOFMxC7nb7vcawcXiBZDb12ZWKZdhS9Gioah0%2Fkg04Ox5r2yxtTL%2BI6Y6CwWWrRnNAS06B1rRVe%2B34dSGFUz8kzCeBIUg%2BrSOf9rBATOJDtQU5bXvJZmquhTRjN73MqjGMpqdxzvKlnTtf3MOLClccGOqUB79c%2FLPUPBIkPCR7M2a%2BGhTYGTz5bVPSoZMfQyr6QqSRoRvzJCf4J53BfjylpG0xnVT8gglJMxg5%2BBlP2%2B%2BraUXKZE8HdSbz%2FWNtI%2Bs%2FcbSiZjHcStdQzb5QMlQicPuvo3IXieT1po2ODkRMcJiGuF%2Bc08u2JVWjvGlSlM1iq0Qrs1XiZ3S3v9B8qWTCNyMJtq5%2B1z4odHvUNogoKHc6TW%2B%2BbY5nV&X-Amz-Signature=b72849288de5c2332fa7adb0acdfa181e796578eed22f235de3228b21a57c797&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


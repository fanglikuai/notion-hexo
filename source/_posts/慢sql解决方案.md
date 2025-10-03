---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TEP5YOQ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDst0M%2FwsLZxb6nEr5UEHzJYL4hvdQ1d%2BJQzUHtyLbffAIhAIehHpFPM5mHUiwsvhl7qCLbSOnru4L2LJUUoTHbwk4rKv8DCD4QABoMNjM3NDIzMTgzODA1IgyTpG%2BeYXrXryms7qIq3ANJbNg2IyhcNXu6rFkDsfPaadJ1%2FY6ZpcMaV3kwNbgPARtWOiNDjEKqBUxdpTQciCWbdL%2BTxlsILafnYaCBdEIjSumQ%2BuCOQVRW1HVNqiA7%2BF8u3gl64EW9ukbm8oI8o5Vt3JlYt%2Bg%2F7Xb7q%2BMdpvzeLmF71ritO%2FmWAl3BXM0mMNFi9QFzMRtZsyBWZeMZrc2lVzj9LaAfEOHlsvFT5yj4Zl9N6h3ORavcIy%2FDnYfXWlkPGTvhC9OJIz0zlL1h%2BNiJXdJt%2BNMcIzMIo0rPS9WC6dhAOzFpCGtJld9Qqprvv3tlXUGOAVt32txguEkuYnxmlblwgyE85HQsCxtyv2Y5ThlXXvSmVyXgFwjjmFayCnGHM68rvOQ6M7EaNTUziGd9Z942A4zFCQ%2BxYx9pmRV3JdG38GLqBnznm88lRiXHVj8ueIGA0Hr%2BVTq%2FJIlHabw99gbuR88tbTuvVOIhpkCOe1u5Or5dS3byyKhPA6SBomW4TKIyXaAp9%2BwvSJVK4pu%2B1AAv3%2BleCHa8k%2Bzu4jmBmuPGhP358SaERsOM%2FDJrvBOTxHkFuFQXZsmNKPnZNctu98SICnYlu%2FcvTG%2BfFa0%2FSWcSvE%2FcxpZLCIcWvd6UES9I1%2BjZuRktzzvyMjCXrP3GBjqkAX31CzigmhpWZC7xiJdTviyZwT3qlWz27FFCT9ROA0pvI7VQYJ%2Bip38DcSE3VVtcNTKO%2BXqlkTj15VockyOJrqibKDYa%2FVgHqeReJHvqvXjLyG4p59pqpigbhVWGkC496SmdWlJ%2BBYj4W9DiIqj%2FqHQsUYE1vOuo4YbSH1SS4EmPNlYXppTz3eka9zetmVuWlICs3KpIVPatWbPAEkEyvRQOq75m&X-Amz-Signature=2c380d892d5332357910fa8c24ed3a58c9b94510dfac949e26ab2057bf19fa90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


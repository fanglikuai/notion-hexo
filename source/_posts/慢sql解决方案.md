---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q7HG3LZ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCICnE8TUCpLgCXyFWh1tdvm1NJ%2BuJeQlmIl8Q4g4hhYOQAiEAuYBbdiPNbTzI%2FNykqbeBMRbClLpLOGZZj%2BgyD2x7CqMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC8WupoVncujYseEUCrcA%2B91FBdHf7Th2ZwXLz2qWD8ZGZhXBjWv%2FsR8QvyxLkdc7NnTLLEe0ogXGqHcdPlc%2BbdYDeZKd7grzbbkaa%2BkKYzO586rAWbquWHT%2FEUQ54hwm%2FSzNZfjGWSo40MIY%2FUa9WlUuDXvpZBfZvkQtFblwV%2B%2BnL%2BKDgGbXzuIOBRk5erJab%2BftVGE1O1rCvgZoAw4mDsnWSiI9e4J9bY%2FM11BGsrlmeCQF6QXM8tkga%2B075CB6Rd%2FHbbbryjUPk9DvwoIXSYrO7JAjYgOrpIuJuX6cROCTwVxI0E%2FwqIIPFFBwjUFrABjBI5e0zhlUyX74%2Foy4KUF4DJIcicCoWs%2FTEcgRqzH6VJjTdNjJSkIsa9qfnm7W6kvGtCxO73MpzFxx%2FWPjoHD%2FlSGxGkLoZ3zp4AmwjhX5fzR9%2B2joe3%2FMloq7DJX0kvMLCHFzST6TM0bjzWI2dokqjMpdtO1F0lOlr36tinbD88aGJ7kXr9jFEPu5jQTcNo1Pu1kJXTtJyJeSDSZEpn%2BVMYm7p17o38dS%2Fe%2Fr2bn1kNJNQNs6vNd7I7CC9GMQv72QcKNQxjWtOS1QIsSx7Ziit7QIXBTI1uEEw2hhF3GPUoEoSbPABiE2w1uzHTpajFMROogjVQHR5SHMK6z6sYGOqUBwSvqEAhD73NhrDZqmoejU1W6cQ5DxL%2FNgY7PyWuvRkreEv8uWRpE9KXqjaujGD6SsNy2IAEyW39BJbjIjKrEvx1bASyOjxeAZKiD7LV6DTUL%2F0COPrcxnb6YUN8j41p3l9zyKXQDF6CxZRaYvdXzJwQ9GCDTFtNV%2BX1e5f8Qar3KndZ8d16JII9xsBx37biBiyNvsG2nW%2BZzh%2BWPELdiPDPPS4Fm&X-Amz-Signature=02d7545c065254a6ed723f7d202ad5bb1b9fc779782f92c488ffccd6cd8df5fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


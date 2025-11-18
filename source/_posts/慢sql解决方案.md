---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y57GIUZN%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQDj6YwHCADhEw2BidYSrvSsO6kdI2R3DQnZHz4laGfBGAIgLLX%2Fkgs3DRDXWcMt7A8azL6x63rAAS8S7YcvEfkrOtcqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD0woJp81FQDAp7LeyrcA%2BxOrpXWaMb2rQSYpUQTcKQVx79%2BO9MWeOsu9yG%2FLto%2BQp9vNqw4qVl4HZm8ZJZiRKLNkz9%2FcmttbwK9GO3DZ7hytbYyI%2FOZvY2daHrsC5fCb820OKDrf7EFN%2BuwciuQ9vB99n1WCaHvT9UJUHqi7CNzOZyYIBTuAiRgq3hES7cVShpm3hHBOjid2VpWDhbvhDEanrUVPeA9S1QZ8ld0vUb%2BcnvqLDksuj5Vvc3%2FEji%2F1jM226ONtedwFAta6beTTFDzcRkC4t5B5jQGyCurkMkWbunz%2FqGvguVUWtCAC5Op4qsgYzuqD1Nh%2BqzWrg69eLj1WPYfkdYp3%2FHmpLgZ7v89abn9Ra6sWyiLSeGFwUjBxfnswTPbWauNxVdt%2FiFx%2BhrCvsucqm9MP3KxZBDWFotqDw%2FHdbRSXQKsFGFXnv8%2FszlaTN3bToeRhb5axLD5tGdpsbSRmEQKQRTG7%2FjiWX%2BuTMmC5E7w%2B5QToXGng8cKeWTDBtNcoDyJy9UePeTfRUHcpV%2FmXpI13Ox4bmAFbaHGJqWqQqOn0r1E%2BWPoTAgxgBR0Et2X1BXuXp%2B0uGzy2bwMlVZ7ODkeYro6K%2F3TXPAQzjTDIBddI%2FOKWNLuJQYPvznyWnfUpk31iohlMKyj88gGOqUBpBYlMKfPkLh0I8jsEGFWP%2BDYhB0n4uxJnohNv6u9Y8uypDPK2vMmbWTV%2BRk%2Bj5H0xsWpSg86mcJaMFGW4huzpzwKL2TEIbdBuVfyrLItMksHOJ2qizN3Eaec9arlbM14p7ezJYdPLl6QVVlQuHv0tupyqML2rOgbxXqN7ytdYze0ii9MahXs8U1VysPqyTsfFvaS6cJ4FNGbdnwOE4RKAmPqxHSp&X-Amz-Signature=99336070a1cc1d52c8b72592c61dd7c0803df8b995507cee23fc1a1d4ce105c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


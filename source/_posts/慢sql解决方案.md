---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XUNUB3S%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCanQY%2Fsp1g9IWdhEm2%2FtpdYDnfBvB3levGoYBTU2odVQIgQiGDz7U4V8u%2F3Kdou5LMvhjSGURb%2BSE74iGU5uUVK1Mq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDAe%2BKzFEL8gcWLa0ByrcA5vAQeSIzk22QTjqum%2Fs5t5FoAHluYmEi9urxGq0Kwf6Xv%2Bxlma91XrhZwhLraUOXR5FVSkCXPu7w78I1St8nIrYlnNDqOOkr%2BAlRTImFb6b6iE3ED90dW95vLMy7559Mv1CePAkwcsuA8s7Vjr2rVtwgwSbSxA6r0AuWozQpKylFAEsGKw4dZqbqGuhBoAySHenae8MhDdySyNIy9oBY53pUvmZEbK%2Fm%2BmWsQ27uzxLZaT9dPGLAaso5GDkvIswuQI0O2cKBwifaEeX851xAm0VNhUOylSescxQBeSe9VQlX7qiFkx0Rmy7sIeA4mM1FSkAlFoZwqPFYqlmwp9H7D%2FAGNUCux5JB423iigZKwXnkJHg9Rfh5FtyTWGrVo%2FvjOVGXBRxKpyLcxzjqTmWUdosXVFbFMI%2FoS0NnIvDvwehLC8BfkMVdzqkcyKsCqzyXmbKBPOlzXGvy5xZgg1d7Ii8HNzjpRgVDz908am%2F05EdK8243kO%2FGVXbbaPlUXldKAn8VmS8OAesaMgK85aNzBo7Z9%2F5fKfhSSJg%2F7c4e73TMzD1gNYHbG4mgObh0PjqXsn2RjXFWxcQvicRDNwxR69LXvikt%2FIoSEQ%2Btt%2B%2BnwWteZz6lfAbMNSIw0%2BYMIufycYGOqUB1TjwjISG5iTzNAylGreuyrgRD7qsDjGClLewSoJ5INmqxG7JcnIoUp%2BW7a4Wf3gf171CrUsjCcH1kppRHFKn%2F7gxiQoyy2bSIZg%2F%2B7oUMFnNeOKYj5QuUKxXaQZd730iFHVRv%2FCOkCPIpz52JHW6lXGajTCxVaHKG%2Bo06bWj6%2BJxegqY8pGe%2FqqbM%2BNOGbSdRgokvYymfq8inZ9qqAdm9SHry%2B8I&X-Amz-Signature=680e04a0accc86299fda60fe260ec48d07a07b4ef6280ae8fba5f60154797fc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664JX2H7HO%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAdTTRSDqzavtnuKYm%2FKmAdfw6ul2ayrGcoo2XLUeD%2FbAiAVTpS1jUE4kTp4qoqev0MxG5jsZ8xewHOGKO1chLzisCqIBAjq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwRMy26Zw7%2FJn6rE9KtwDjJmCvqpjxSB4ngzYHPz6imY7kAeH9%2BtAOygA1G%2F4kyec5XFG%2F5Y8RfhbtpW%2BTsZxPYNHodSaQUDKq003Uh3WtKK40oizDReTLA7yaXF7wqg8k6VZ68psX95h6jHvRSYp%2Bbqd%2Fe%2FZ%2B4qFujS2O8n%2F3DgWlcK44rMYq8dRBgK0IIx6FdeTJ0kQhimxvngV5O4VHDaeiJ7hTwNKK%2F8HaoD0ZIUZHnjHhQySwjyfUgdPWQQfgFTmMLXjcTExJc%2B%2BkIcmDcVZS0x5reWp1%2FlpO5qIlBGG2%2FSxyOGvUVrPc2FxMLS5MUvzrJt2b2awV3760X%2F8fHORMWw5jKepLptP62MJsWlCMWsM5TErXq4qCqwuuP%2BAYkDvrcYJ6TbmCwbpqybS3tY0g6Q0KuwAus1iaZLIpuOeY4rEiIyBbWXlToFdxAWS51uYAPN5oJj6Zklh7zTdpzcVCIoccR8oob2066%2BbdJENowaLS6Yh%2FHCC9GV4exdovqkACY%2B3UirCx4n8YmueOYcuKhO0qrCWkyXR1oGRPjwa8PrXm0eNwS939I3R2mdo9ivFMYQRHIRB69UKiM4D2U%2BkhuKAGAYhCsmM8e5CiW2WV4VxACALg%2BHBV0Y9CNWovIy5H1MZhnq%2Fu0owxNX5yAY6pgFcu5WLi3XqzZpCwnI7vnRrMXfPVFffUBJ8NrF7UHWqzP4dHVI0XOaSvnUWjM0r6ry5XrxylirnQXPetAOziDSqqO4KduI5Y%2FkznTo1pyGp8ziOel0V7ZZEe4xpDbTI8FWTe1La%2FoevrGTQEOB9US9M4QwXnucx245wD3Dw%2F4Z1ptny6noSNsM4olpsZLyj7k5kS89xuRCwTDWFKkWFEhYNr4QeQyB7&X-Amz-Signature=7f0e8a76e0be593e9123c3c57521e5f3fec7ed261fde7d57b1fdc6502648514b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


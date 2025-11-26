---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U763VWBO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFL3BlfR91gZrkq7ldPxKlZ5N68grefRQ1U8KvtrSlwyAiEAjpLdrtLdAgMjn5ZOGlQnTk28x2UhQimZGZCetZGuAGMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI9z5XXu2DhhS3mS%2FircA%2BzvZCWOYkrhzUhA27%2BacHH04aSRVfqpsxbIkpLa8Y8BO1iGwzcAz4Dwo9DNUxQRKjGecrUvr7YbrwBbEI57Va8Uw21ETOw76C3BF3d9%2BR2TtfCfu1ch12GpDR1ZZbUzgqM%2BWrqOzpX5cGoqJRV6FsT%2BXZun2uMbME2NPa%2BZ0xl47hyarb8DK7FmTVkslkBmUE8lAYMUH9ElG%2Biss9UAUjF2NH%2FDDrxFH%2BBwqNLQh%2Bv9pvhj%2Fmn204d2bGjSH0%2FpDXK2MrFUH3viAKDcWRwuIffj9GKhBeCJB%2FJiwcqAxyG3ZNc6uWpcioryH1HsFvIQcAqEWWWwdHl0yz5esbsU77cYDMiD8nWe%2Fa9WvMqLUxP%2Fs9sR5nxXrJ3T7uKqgAtsWVzVXQ8d7AuMuf3OfjQNXf2E%2B3uELZAT7XaaKCIlT5NQf3fyIWsLC8MSjZg8kucjLB38G57xmcChVQlW76ti5pG4OPi4UGWO8yVnc2eBAFeoaxRjuPTBfuAhFCLJDDwSnRMiWhsTCR1n4WcH9X2ehtF9SrOkfi3Fqo7%2FMEtDJER%2FKmsZTacg12VUcLKBLsQYBuQu7iEZ5j%2FnURaFyOuWgsEf4ao3wnlHSvTb9QD%2BWZgAPf7scwzB2YdBpSheMIyonMkGOqUBmvSSjSNENQDW8RTqulRIo9Cdud%2BiEeH%2BnLQPY3iTrujz%2FJAaZrAAYGgGL06%2FPJIRGzideGHtk61NwfdMc1bJ58owJalCiz77wKxxc4PbJNewRwDsHfhOn9WHfOjK5my%2Bxn6i5YD1HdxM25Dl%2Bv6M%2F35e7wf3KXKlSWpOsVayCIWhN0nMqVyqHEu3kFqQxSa003jF9WOYGN%2BOvFs7wQB%2FA84cxDtE&X-Amz-Signature=0068d59eefe88619565b6f382b3ebbad8799f5ca51551b73bed932a76b477d31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


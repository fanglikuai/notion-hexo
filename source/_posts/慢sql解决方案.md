---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMJ6A5T2%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T230049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQC8w6c1iGEyG7X21cF9YBgQqt32iclBUgpyCVmHFWdgkgIhAMaz%2FXG5e4sSORNRMXJxRe5zvP0FuVR2md%2F1yyqzJNbpKogECOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxaj1SScgkkj5qAMYYq3AN%2BniraGo%2F%2BlzQGaW9%2B%2BmT96U3yTJxhklGJ5X261QJUcEzdmbu1FDoewEL92CdJK4yYuP%2BgYBbwGu%2F1lE%2FXWh%2BPHoi1fwL4%2FJdXa7gUVUDMqcV6VZvW1W9N1XDAHPmawCpiXn4A9z09AsbwmuCqyRks175xwG48u11n%2FDfCGqhk2ToDO%2FXE6oIyLTG7RvOm8rGjhRhWenOj0vtK1nqg06BfTvoSLdWdQg7H51rFAzFyyOmunK6873PC0tI5U1JvSuNhpKfstv6ncNHCH5%2BWpsStAM9PWW1wK1c0oAo1pEsgYYEVOIEBqHk2eB3%2FjHEhPHmdXKf7b6mvS3HZZ36oekVabrwPmE8tyl1BPiKI7S7e72bcBKkM3W78%2FGv8EtHJtJL2jQ6fTJzHeNjZs4tuYl0PeyDW4H%2BK62uUvxUUYIm0RuZ8icbVa9EQjZc%2BpfIz8fz%2B6p9jDCILGVLwCMyRgP3oGSqaUmDpui06enIIvoYKbKNINVb%2FauIY8Z3SMbg7aEaxxyGMCIzlPgU1dK%2BDIbm4WzQyfSD1EihhoDhYbYYKAxXBVuyfcV05kP4uWoR%2BZwY7vN4czbWCeMq%2BPC9YGZcHAE8lysi05%2FNy1EmRbEJD51fJKYgWQIgIxfCxPDDNr7fGBjqkAXrf5DmHsk7XoI49WMb8WaXWqECpXxKzFQwZ6MK6xIJncfZcpStdPWIzKLElb9hWtuS75%2BgvNrLvK03RJoS9Gum8J9pGLzzvxdopJYrKb9DwJMntYRg%2BJOJq235hOqVTmTvGfW%2FVBYRV1BnAMk05IbSU2cs3r7h%2BG9KR47pXyfAq4lGnbleCUXSo2fkoZmtV%2B6hGjPl5%2FnKElL7cg5v%2BxT3KXus1&X-Amz-Signature=786e73a331f2e2cbd5eb9949b298c2647d2431ad4248b5e746b178725afa5751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


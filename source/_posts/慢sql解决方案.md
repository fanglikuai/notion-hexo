---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RPJXX3R%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAmHF4JyVoRle4jvqCEVsKdlbYxGAvjrOJ3E6fkZW9iTAiAYVX9f5G3W6f%2FnzLjlUU9qBo4L65yzLrGOn24yKdOm2iqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FVzJbdHfUAHJE8WIKtwDOuiJd8XleoMEJU0ElIn%2FYtF4vgrA1RMwBD7OouGGcaKnshngOOXptMsKAOLW6FJmo8yUV0I7cSqCOF5ExGpD7HCDRUtyfVqseyrdqAFKKUP3hUi%2BVLFHZmpSG6JmPmYNOOfYAZQ33nKfEiuIN6Fdehx9JhzosdNJSaWAbDzY1MxCN5G8cx9VwKToEQ2wfp0JsmlIH0c5Zp8sCPBb8nqFh4S7N97Q3rXDEoATIbK9W9pBdNINq%2Fw3SjuAjzaIYT374zraP0vbZLo1An2EJHbyplP3smC3gL2JynYYE6vd5ISrfYrC0ImULMtKAEuLJquCaWCh3rCfEvF%2BfOd0PH0C4twPXO9a%2BQK8bnVPYPs7UjyHiDsNDbiy%2Bme1mAzeZME8dLWWG949gt1cIShQ3WkqbFlcv7xio1LP%2Bo%2BW%2Bsy7gbPSo4hfs96bGkY0pP6lDq6mcFkLHX3EdyJNlJFUibE6ZaZ6pamkhiwWEn0PiCCEtfeHV5nB5pXlOYgeQBGiBch8jXx6K8TL9b4sh%2BVtQSSyVnUxQfYluhmVW1%2FjjoTxc7FeFjT0aMxPf9YFuOdqh%2BfKpd9BZPmGcsSISUf7qrODE4kYFQfQc6TJE%2F7WVWgswASI%2FvC6%2Fomd9MrsBa0w6MTxyAY6pgHDYy7cHLbFRb2DB%2FreyLrDiCFi%2BLu36tZvhNFC1jS%2BLkIWl4qKlY5ZyYuOlQquNbqX5U4S0HcyoWpYoUZNfbUAiPO3o0e%2FIUa4mvFzUxqnqX1AIWIkEBi1vYxyMCE9IsGYIAGdvippUZc2NoofpbpnVNjKUbxNGNu1R33d5tYX5jof8tdxOMB78C%2FJ8Bpt%2FdZYhaP%2F6cKB0nawxXKcnEfSP%2B3s5n3B&X-Amz-Signature=74365d89299f58ea6cdcbf2e2f4e637652d557b481f746e380ebbbac2a847c59&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


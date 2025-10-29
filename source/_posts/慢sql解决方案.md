---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WTXK43Q%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIHjqFkSOQMzgX%2Fc9qncM8umQ0yeDQHhulgR1pKsqRdM4AiAXV3isHYLD8Fj79ihth6w8asy8eVT5UgcMBH2uyp1idyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BDrK652v13jYfz4zKtwDK%2FVmB3fTId3iJHSC9gZNoMaf0DZlP5qM2vgB%2FJt0QlT80B9VqpCp3tCIAy%2FFsc0WHNXkLht60WkVcuYITnL4kWarzL5pSU0pvnr8%2B4kqWWprwk17hGz8RME5wIXwVcXZ2h%2FlvNhC%2FmZP0KS1uxYS3Y2JuTCwy6oVTVkQps6BRtkkreKaBopIRgUZT5M1d7TGR5WyQGWRvkb2pe96%2BCdhqAyNdI7oDzRJ1WVLEkaOzqzdbRbs7SokSqQtZ7qa0lV%2Bw9GodHv3dtQPtgh2bcwc7q%2F%2Bf0uqy5VTkfsMP6TC24XI2Gjoij54Hep1HpJHpeQI5O668gtd7UjzE86oGl0rFUxpFr7uomPKl%2Bp%2FGZmHj9D2eOmX%2BF8Yr045UOk4fC4xhUN73LNcbSN4IY0va1WSdGJ0Hj7Jzu1Fax%2F05m7ZaW2FbfW406E1ZBTE%2BE9TQ3xlLBmB8NABRjRq%2FX4edxsL7dL9SfwaiR3TvkK6vBaT1wZL%2FutsCW%2Fmdh6FMgD0zqhnyfMcV4HYxA3i2hH%2FSETvxDiN7pDPIqBQJh3j804HQqza1t9co0WJARGyrt6W4SiYuuidAYcCwXCZ6vTpKOoQ7MpPoCn1tu9oaB%2FDvSWILcKVxKMKnL0nNGmfJwcw0ZOFyAY6pgHxg%2BV8VZ%2BfgWIkcMsbx24Fjx7ztPvjWIDwlL6F8sypKH89jNgpUsOyOWms%2FMwDWe4jZl9yNcBokFXC4YLLGXFPyZZh1S9Hdj%2FfkSy82jZrT8fYfibuQWci1vlP0fOz8maVSk4XN1DK%2F%2FaCsKiWYkTTDssuXl%2FYts7rJwTrmQEL0Ltg%2FSVvU5qY%2FOupbyzGF%2FCoJWCb2bFY08HZZD5ji89JHsWERzvt&X-Amz-Signature=907c5afdb60aab879692a735bf95b0dd0e86d164e3ed1ac29d91d0145ac94bb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


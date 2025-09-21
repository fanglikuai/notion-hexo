---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ANEEPBB%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDubH6jOgzMPc9w73HTOqSYMpixrzhtem3iaZsvw6SCNAIgX%2BWBLy5jMT83f4EJGXzFFTjIYtbvT0U%2FVCKFNdJgw60q%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDNWYWYSE45s3HfXXmSrcA4KWy7E66SFA%2B3LUGs5j3HXtPJmlK%2BBopF3u%2BTHnXJ6KQpYOmS9kHN95o299uSXHImpZexqY%2BBY6iACvv54iSXGrAqMT8xYZLABP6S12YGyVBEGeCS00WCP6iWYmq9DOwsyRCTzx2gOI3lGf9za6kLgbKc7Wr%2FLmIdNcOG21ohEarwp7D%2BTsJcoH7wDfxHxErZEdpNOSWmuYvq%2BEY7UsDP5nAD45vV7a9QA3MQPSWrd4RJEH9%2Fegprd0bJuWA6%2BjqiAVpH4U4rfIruePXwXwwowH1wLy1ttX3tm5rCFJHdz99XROjJWD%2BJE4UsVS1jiQzQ%2BBMZcr3GNl4gQ%2B6vfTFVO0N8KBY3v6wLQNc83vD6ZaXeQDQ3BCZcFBqGHHfgOZGKCKqmdKeWHCfUAUJYBkK8ViW%2BUC%2BxAqdXtHdiEQKn4gx9Xo515o6qTQcNHs%2FkGLQAt2JSto8A58RzqUpzJR17ylDsE%2FJBKAaDAMzstKZmLpze9WS8HGm%2Bxj%2FNaKY%2Bp9tjGU3%2BcVB2p7uJsfL6T7SsIaJwDQsvu7UWslU7SFAFoeHxZmT9i5yAyN0B4RLP0oeFDIVETwloyTdGZEtGaA%2B5yyMdWf3AjHdBOXKgTE62iXcYgz06e3zvSwaoGFMKy%2BwMYGOqUB1wTWgT2mxVFRsza4sjd2BIhhjJMjpu8LiD6fS0t8pZJXnJm5vhCRn92jITmL2XEg%2BEZ4h6FaZR6bAu6FwhyBeQsLj0Evrgtq8rlU0cJimVUaKe2w%2BSIuVT4uNtw8IWtvFnzzZEcGIpmuVOoRLzMy8kj0h0jQE4p8Qncb8InboDOLDvezcJp7xF89tYH9qmFsd6s39wJwAJFzAmuEIhLF50%2FRW5pv&X-Amz-Signature=60be286c3af901b5dbc258f0566a2e01ce80fdfeea580f93b30961f58dd0b5a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


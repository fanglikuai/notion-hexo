---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGKJEVGH%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQCUqIgPrJvcpSG8SXOn9U34giEPGHD8H%2BplKw8H4ACjkwIhAP1FFa3B4CfLSfWxvytChbtNAi%2Fok%2Bexe%2Fz6hOCStwnzKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwUZUYa1YUo1YDyfykq3AOjHmS3w22cr%2FanbLAWNSqqnRYZNwAfx41zC7wDADayNyU35bKkYdGOcD13PW%2BlAiVNKmwTuh6jdm%2FRcbVoVDi%2FJcb0UDn9gEUviC05zxjB8hd02Zi6wgJBSgE3wITNdRkkV%2B4QJwf%2BZEgGz7i%2ByZhFfBwWdUGtp2NydAUxSa8c4XCKsjK8u54AOEl83k58xTWEAW3oTpYuU4vA47Z89COJ3GP2hG8utRXKS%2BPLRed%2F59DBioGlQNhrmWzLzCN%2BUw6igX3%2F9tJg7pmj%2BYrY2UQCH8%2FfNuQctPQs0rnEv%2ByD3ko6LdnGrcPvpyAppA8S%2FgmEO6Pj1Wra0YLs0vDqnuEpMj5ClcMrT2Rq5bwxF8fKzHpvhrno1Z8NU40FopnMKAuE3CHgJbGIBFFZUD%2BGofeELn9QXZxfpoQsqBh50tK4kiiM5F8dqhRi0nnSS6ADG0xV76lIfYpplQR0aWTzjjm%2FkTX6HfWDPPJgPlNkhYgmwUPzJhbI%2FucLwtnyp1OJCMqCEfz7%2BZMNhmkrxMDR7gRYeXlfV5cQMWbo%2BfrxFr1xh9zf8FBj00jhf%2BqfHzwRUShp8vF1WbUbQS%2BIfp0hxfABeGMevXYLMJ39p%2Bmf2Rpwdl7f68mvDm8M2a%2B05TCW3ZPHBjqkAYc1C7gUsfnO%2FzvCuPbk7UTz5PpS4TTNXgobfaUSOKAVUVj9LppK%2FkVdTAsnJ92JVf4pF6oWlNdNam2ViLfYTSNHoRL2IpEghk6o1Qils3%2B035COa5fL5eCBM3yYJKq08Q96JScSmanmt3iDifSXuJTYi7lF%2FQwaRpcHIz4fJlitjqBKUAXqFkM%2FbGo52hRMWA3%2FvA3PvLF2sIYjOvCCGNQ1Iti7&X-Amz-Signature=028a03744d248590c8645d208647419d0f6f1f4ee577c88d3990c594295d0bf3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


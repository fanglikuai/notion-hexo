---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IDTRRCA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2VQsUvQtPRKst4B7xsrHeCGqvIihupKTM8cIV2mbioAiAPFtaCeooQyy0dB8c3zDj1xLs5fOsLQBWQ8%2B7t1vgC6ir%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMmwW%2FEezX4eNp%2F398KtwDa5%2F%2FoXJzMD%2BIcPuBFRIbj8bOJXaki31aSm0w%2F6K9mUMTdYKg52eqYJZsqN9C6MhqEEB828pW4M%2BgKoBuacFNncCupzVWNyAbX2xYMi%2FOiPjl3YwqajQt6nBi3uHJBiV1tKHjOLZ%2BCLPUKRZOziAUTDRj34kxeVtFgSdvwUAfCDRFRzYLn5JuFkjhRGW%2BRZ4X4EjzkC%2BkTJNxmQiH7cbLOaM6M9fdlUS%2BlF3EpQnHkyDlhOAsIzjVUBgcY7dZ0MzJgrzV4b%2BmM3DqIeAFekvJne8qxk%2FOhSGSRrWt7NQQfB4w7jY39DjCGBAaBbSLu5kwnq2VVuU57XaGDpyPa6hYUxKrGf76ai%2BGl72u1Qoh%2FOKJr3RSKEbyD4PDrTp%2B5s7rW2nKJPWaqBoC8%2FbUC8E97Pk4md0yOBh6J5rml45%2B0hVCGDHUfsDkAISj%2B%2F0LCYsmCLxj6DASpTwN6pcTjK3NghlayG5WLQMy0ExVxZauKRoiWwaR8p%2BRXgxvTvgVderIBQSEVn0IsEcA00mc3gG2x8fSPEcS2Drs9cuSkAx%2FUjAVmBV5eqDD2VOMMbZinrDx3joG%2FZdQ9lld4Vcv7bP6bK0%2FrwCwD1AZQFiULfrsPsJZGz3dc2kjN9HmwPswh8mYyQY6pgEeu3TQkTF8NWX1nnWllqpI4i26cLdGu3eEKZW8gcKmaSP9nnDUeTkE32vjasGOZbb7w6hagxlBo%2Fj74BhJ6ka6XJlPRxCjS2%2BtohbtBwmUVgYczl4KS53oQGXYjnjho0BVprtYbmKX9y2FJzMwiQF%2F%2BJUK0CBmmPMah7bcqchWzuS1k9Y%2FIBSwlUymYYk40RLevqOPS7D2%2FILDuFvorwJO5%2B%2B5IMH5&X-Amz-Signature=9e33d8c1f2bfe7527d380b5f1afbbbf533a7218ed7deb79a79a95252f1d9aae0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCR7GQMH%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG%2BdbWEAF8HQoZo3%2B2dOJFPj2L5MfY%2Fzsi7pxKv8EvMMAiEArdYZFhUN0ZRjQpKUuXR6%2BsJ3Oh1HtfBEwsddVj7SzxoqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJiQbd28TClTd83bCrcA57bVBOp2%2FvUAmzpBc4U4Gqr8rjItTayqYLKwDcDViZMZbbOdnBjc4lllDXrZE0Bm8Kh2cOzZlP3vNJbc0pE5%2BMGjWSzwMgomuJ8LYbxCrodDrYK6nPzzbzltGyMEXsi%2BAyIsDcq9Z2dz9ksYxFogU1e%2Fa6Dr7JmFLqgCNCFOG7KMr%2BpmV2gaO9WOLI%2Bkr1guNGN6O6FDvjjFbLpSCWWhYiaHlZTDPx9Qn4mYQK2E1WRnRmXeckQajFx6XwYlgNT%2BdHyiEEXjvYybj7JCyRxC47vn%2FCUdSJP0lTSNNfQXCRxLd0R6wOYb0qeJ31jILXZSYYDFi8y3hn5SFhmurBS5ioygCUMO46BEvHGv3tvkpkSkjOipGcqlNu6lqdTfuI%2Ffp%2FJ52zmk64hxAFRkxlWNmMVzyHPdWkEVf4jeKtGU%2FBCB4Uw%2FiEakl49t6AgDVaTm%2FBKhQ%2FTAVW3VoTjd9p5XdoT1GtxGVRH0QT7uAGQRhRbhH8x9URcdqiQTTSgWdOqZnEbkXO2NaCT7qflUSreKL%2FGaZq0KY6TdkbB1c%2FuYo6eu5qM4NYzWP9xiguugdBTtf2dSw1hej5r0coAO8hBukDKMpfmUoL8zlSL99AMTDBpak3LyYVq8c%2B3B7pQMNC5sMgGOqUBhNSgJFLeZWlxg65m%2BgTjUwp6QL4Jp7F%2FgDBPXIlIrw7Hy7scWFax%2B6dFBkDO8IliMK7tWYlHGq95wFCK%2BNKZbekFfv6sBZpn5orePJ5AYVvEzZul93tIomEcR2Brz3nF7C5uzm3TLsESqoUICkWSRDLMM8bU40Fn5S7pVmleDXma0vLh8pUXm2DyeFgrx%2FtswzSkVwraWKuBJT5oU4qdXaSsR3sI&X-Amz-Signature=13ef501db1e8ee2bf3ef462dc5d135186faa48f89b2934446da99c272383abed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GZYHXJF%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGCrW08A9DgT5fUZjWuXfeO%2BXJZedPDcnxKv%2FHeeR42rAiA%2BNa3hq1KuP1G81soLaCJg1N1bauxEFGkZAoqu6FDBIir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMepR%2FUGGEnh%2Bk%2FaK7KtwDIAnEMEPnmvSw1HcXvt38o7igUxKWox4d8%2FE%2Fam7eVD0HfBZVr3LsJuDyFMssHHsxFydiVbAkhZB7xTjfsSq6wytFtmdela3Ks7DCz9SjN3Bup2jX%2FmGCEBgjexZvDl9%2B0vdbrtX5DzPfbQ62CmveE09rsOsgQDNDYvgsij8pkRjxS4D0RECu6O1tU0CSBoiR6m%2BrgvPfgTXEHk%2BFwsAXNfFvsSARNb8OMUr9y6AFCNVh5cDQckFIA%2FeBVYd%2BS77Lf4TNOsZGVhGHNskeY15xUw9b4rl1CRjXSIPIE6sXbfareoPjqSXVA%2Fq0WuEJag%2F%2B%2FxFSFtYvb2fjreUeg4RPtTIW9KnliYTDBVZ4WfN7%2FTUFqoKAXjfFGOJvGeSRyY2sg9TsqhozfzbKBveoT%2F4UwC2tA8jaHcLCoaeBJOTfgyb20RHsX3cOLmkPD5WcGB0U%2FrGDfbsIpOQgR2EcWkOLHQzXgy3O0Ar6VjrjaMI1udMh%2FBLAdNUXwwoOSjzKXWbZH4KhRbAwe2X6pn39RzEHzDjNMyNCG%2FNietDMlnawzsQzP%2F%2B8GjlUu66LAGaxTD%2F6iyBQiMpHI2e8K6jX64YK70%2BEB5BnTBIH29gVNwZgBQUqvqre7WpP8KB%2Fq8EwhN6CxwY6pgE8I8gCjokdpG3jTYbvEO9X8BLrNs6nH9SVKhrvs4NQ8Ne09i%2B10mRwPxbEgfTMPy0OtC7ajZegDJXKQPhLBxz7mpbf19d%2BxkydNBIllAEYvSLHQQEj%2BFOj%2FuwOld8RpIWl0%2FWFfi0Ia3r0ZMsW3bsjnpgbj%2BbCGKTC%2Fro1EzikHDZwsxKfDvzMKXFHdn4Pf0uNNTOqAXKzgwVfENkP7s4BzuD%2BjGp3&X-Amz-Signature=c155715c5f895f2fdbf6f6f097b368bd839a6422f005a283fcb6c34d210338f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


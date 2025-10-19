---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665EQXWZS4%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIHSauRAzyMgEUfURhBVcjzJ8BU4XQWbChBGrrLcfCoH%2FAiAOaJlpbAIae4nSaUErU19veHANapn0AWye4C3w2aYYhSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMInPRLUbS07v2hffdKtwD8G5iRRzX77WX%2FcIE4Bd2oGViOBN7OR%2ByljwvxBlZx8E9823oaHNYY%2BjH8sm1pMuVIp6tRld4yCi8xQ3fFjDLNyxZT7Jp2nNg6NZH2jfUDxnljyDGLVMOSHsZVKTKuuhvLmSGwy7auZzo6KgogtlPhRpSrIhFq9vlfiIjp27RqrlGCBz2AI3G0t4df3EAHIkqhNr8UyP0t52c4OS0UExj4NLLw7rfWKUGWSz6AP7Ja%2B1uRnyFIm8has%2BjfY8leff4%2FspnpnT4vx0vdqs2jVs52ObADNX%2BU1dpV1NeLhZlcCtx7LEzz9FsOwP27mAdPZ7VCbWjZ0pE%2ByV1w8suHgWgOiYD%2BRk1DbsEuLuWqy%2FBZle%2FZpPO%2FFI%2FElDxTqjUbwwpi00kZJLAiIWA0B9KpIuzhuTnjlZ6oDEz6NFgY5rAciD9cxbSaGbrsoYD0j5NYYutGG8WnfKeKfbEYz28%2FhXDvf1FlYytE5iPAPYqfuDVPhfihPDNz8InUbWd%2FY721T6UqJV02lnnMsmy%2FWd5XX1A8zEtMnN%2FzkA64zi9otpqvkuZroz62TIRp1xB%2Bp0rofABPYuMRrJACn2TmEzGrbfMbA%2BqHeW%2B7NImWw6TnkrSsTxseUHVXDvEdhFP6HIw0rjUxwY6pgHzgVfQ3rp2tK0JWqQBZuf7vGk2HK9T%2B4GzFvcfA8ND2X3UDwa4b%2Fx96hyLNTtQPRW0sOarEvElOtisr1m%2B%2BRmiJN0PoZiQxpH8xVDX529YrEcAXpvDivHS98G4qezDGt%2FbIBP5krKpSM%2F%2F82bDgsJAXgvUKL3SlJcX%2B4PufgFGTXdvXADmhG33D6iVNQiBbL96%2BUKwPgFZLnBvvKKDbsZ7Cqih5vyW&X-Amz-Signature=da4df45720732a6b2151709c71927a75701fefddc173426ddc3daa3f0ada062e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


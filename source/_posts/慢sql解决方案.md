---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665EQXWZS4%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIHSauRAzyMgEUfURhBVcjzJ8BU4XQWbChBGrrLcfCoH%2FAiAOaJlpbAIae4nSaUErU19veHANapn0AWye4C3w2aYYhSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMInPRLUbS07v2hffdKtwD8G5iRRzX77WX%2FcIE4Bd2oGViOBN7OR%2ByljwvxBlZx8E9823oaHNYY%2BjH8sm1pMuVIp6tRld4yCi8xQ3fFjDLNyxZT7Jp2nNg6NZH2jfUDxnljyDGLVMOSHsZVKTKuuhvLmSGwy7auZzo6KgogtlPhRpSrIhFq9vlfiIjp27RqrlGCBz2AI3G0t4df3EAHIkqhNr8UyP0t52c4OS0UExj4NLLw7rfWKUGWSz6AP7Ja%2B1uRnyFIm8has%2BjfY8leff4%2FspnpnT4vx0vdqs2jVs52ObADNX%2BU1dpV1NeLhZlcCtx7LEzz9FsOwP27mAdPZ7VCbWjZ0pE%2ByV1w8suHgWgOiYD%2BRk1DbsEuLuWqy%2FBZle%2FZpPO%2FFI%2FElDxTqjUbwwpi00kZJLAiIWA0B9KpIuzhuTnjlZ6oDEz6NFgY5rAciD9cxbSaGbrsoYD0j5NYYutGG8WnfKeKfbEYz28%2FhXDvf1FlYytE5iPAPYqfuDVPhfihPDNz8InUbWd%2FY721T6UqJV02lnnMsmy%2FWd5XX1A8zEtMnN%2FzkA64zi9otpqvkuZroz62TIRp1xB%2Bp0rofABPYuMRrJACn2TmEzGrbfMbA%2BqHeW%2B7NImWw6TnkrSsTxseUHVXDvEdhFP6HIw0rjUxwY6pgHzgVfQ3rp2tK0JWqQBZuf7vGk2HK9T%2B4GzFvcfA8ND2X3UDwa4b%2Fx96hyLNTtQPRW0sOarEvElOtisr1m%2B%2BRmiJN0PoZiQxpH8xVDX529YrEcAXpvDivHS98G4qezDGt%2FbIBP5krKpSM%2F%2F82bDgsJAXgvUKL3SlJcX%2B4PufgFGTXdvXADmhG33D6iVNQiBbL96%2BUKwPgFZLnBvvKKDbsZ7Cqih5vyW&X-Amz-Signature=3ff96a2f01bbe71f76712e4c22b7e5109748aa540fe097e9696509e4027e3201&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOMH4S7B%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQCAO7NN%2B7FD4z0ud14Wrpn%2FA7od2EaYxmkJv1NJ9aCKigIhAPWJQceeSc%2Bs8eBUVhl7IQNaKJfnkRMPHnPgdeGg%2BC%2FfKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxL7O0HNJ285oAq5aQq3AMuWZOHONKPdgp6Q%2Bb1%2BaYqOftjUxkQMg5K1is%2BbeXwIejas%2FGzid6dSzQlnTMTuPBXZw%2FJchK%2FF60hc%2BpSdPcXGp2lIDO8AAhe%2FtNKldhCWHeGx0I35pWEGuVuBtuIa4U0O%2BNKYD9WKplk8EnheTxyQe6YLcvJuT8162gTRPZmQT4kq6U%2FyvOuJMXsNdMU5A9KXGcVmnkD1bpbCNYRUMxUZ0hXcgjNO677OpFLmwYs8DweJphpt06wu3uP9Uwq4bX11CuAbATgNfwSqZUX6yeYKmurSDWxi%2BmGYrN4VnP%2BQ4EMtpoDlghMIFTyxzdozSFdNBus2N8JM%2BZ%2B%2Bh2q%2BizcSUAFBarrcCck4M0VCui34c%2FRwnmEVK2a%2FTQSTMNJe0kpouc4EEPDigGqc8%2BxC5b8Hz%2FmTHmBaYXlMpyL4VAYDvB%2BGs6gYPecxmWQnywg%2BnOTtXhPIXarrPT6h4%2F9EMt2F3%2BoDbrlOStVqo1YY9OfTXt8mOUoXMLzgfuGJ%2BJKCfcvXG0Bnygz%2FyoOgrXcA9BRildXFOw2g6PwkC0pNqkjGMsp35NpqXYF%2BPFjCL0weade%2FjK%2BzWUA3h0ve0Wyo%2Bdy6CsIk4XNzp%2FQRCKNa0T3zYOmopKk2cAgaQVkBjDNoJXHBjqkARJL2WaWuLPohZnNLMLr8AW09SN2W1%2F2M7prGw4nnA%2Fvt2aERPd7eBdKYg3LKz%2FqLcVtnPrvk0jrdHLnSY%2FvXEnu9WitZJXSeM4%2B%2Fgqoy%2FKd7JW9nXVvjiNMtHp5tM56mPRSltuIeubupNEyqhcAz7xJwR3CzOkGaJ5awwlmSMRSNt30cCOrsIJVzNVPYMnO%2FwCahk3hE4t4JEm3flgIAmctfeCq&X-Amz-Signature=987fb91752931cb895bb637fbf7c80d09b743e2df9f7d8c338b14a0afbc14589&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


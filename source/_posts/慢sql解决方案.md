---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPQCQDMV%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDYlcpR0Ao%2B6FqcWS5SO9nW7ewHGu%2B2nQVDO2yNmXiR6QIhALHA4617M7vyp49NT1VNXm5i%2F0xQADoAw3HtrLqYRO8bKogECNf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyDcIGWkC%2FfuTYXLdoq3APsgvYkrDJhLr0dXZsDgnZ3wGuqfGUWn%2FQHXM3Kx88tPhzTBu344cWlQUKEnF05MfBgJ7DaYd9TJtcrybXJ43OlkbrLbSHdetk1kVclWWrEnh%2B%2F4pkBx7ZaLpjEWv9ZO0fKZByzdtNEM1kEGcWv7kp%2BqPCKViffheGAbF2A8ythevq%2BY1NNhYy%2F4lgur%2BkKBpGcCga6wTxUN5VV0hSwj7E6p9yLjBjl9apPr3Ns5kjh68NPUpsmne1TRwj6qSTYoNgq9MPL25ukjhkUlSFvNlKK%2FirHyKQE5I3l8Li1ymEeBQwIEdpRqEr8fiWTQz4XrYMfy1hrT3IP%2BCVLDd%2F3j8fTUgeTNIq73jKFBygFmUmC0ZD%2FnMU4mnZMDyA6Mdem3K9lG8ZuEFCt6VQPXPzxQdgR0Ja9PVZVv95DxeiumKGg9rf8LG5eyij1%2B6SoZB7SCHcYYUnQ2s0k9QNjK9ptPGSA8CjLtBheY%2FupI%2Btwlvthu6M%2FUJUlQ1OuVccMDd5lQbUR%2B6nRw3AijXKxQju%2B20fA18GZlCvyJ65mpdptMIYkwzti7T80K01VaQw0LtjOcqlX%2FAiKH8B6n%2B5DLBzqxA8CIj8t7e4o3RRxJR%2FyJjUnwuwQrai6W1jq8BMCKDDVrYjIBjqkAYKK%2FCZF2HnhmzqRlodNxxr8uTmQRvNXHogGtEgzn5WtNAK8nu7deaE%2BufXnTjsno8FdeOj56sCS1ffBx0H9Oyg4zlfxRGmrjDagPVxhX%2BKJli4bM4iM8TqDMDNdMaHVjPdr%2BgIcPLR3pD6cfXFvKoC47PsVKYv7CsnGzcCBf3JS0s2ZEeya1IVxOHNKrtIk7v72XDC9IsUpSoT7Yow2ts4SPpAD&X-Amz-Signature=dd66f1369883a37bb81c91daf21308f0b8bb90aba6205a2728e4c10f221054e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


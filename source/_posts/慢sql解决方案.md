---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQXOBBEP%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEbzzkzRmZ6B7JBZNRYFZvpgYAIG%2FMVWfpLGGgO0V7wfAiAKoLb7FSQBmGH54CWCWLXHerdMKoZUJjjBv9tFD5ejVSr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMWtjThEPcLznwdC0JKtwDB8VmQ0AO%2F3NkHId4aEDii0aYKXuqMLcpbmigduWwk5SeK2Yd22zNOUbQV9FvlgVPnzU8l%2BpaaKjcLo3L5J4vNDCB6Qrv5Gm0j0FYqfUbKd3vHlNH3H5QRd3Vh7DrjLLzeA0LPMJwy8CJeIYtfx1WSiWOkwpyvvbRkoWLXt27q9F7hJKIIEk8rWhKaLHhVtvzgVQRrfPoU0KSQWLFlYZD33VcqnnWjQ9Lg%2BJaW9tVzLZsr2cZNjP8WCc72SXyjytruLu0wVkbzAPHeeBOIDa4mfSl4FrR6W%2B%2BIT0iWDWkv8Ug2zBye5fxKsW%2BsL8dW%2F5XP%2BuH%2FPwXWbbUK%2FUOeD6yFi7G9B%2BHCV7cOf%2FlhKHCxbbjbEaiH06SV31PQNzfTWi6Mcdpiv16nAE%2BBhrEPo%2BKVxDpFgw1gqQaKh7vZZ0xVWotdyot3WMnhbG8rTUr33QKUiG1HsTfG2UC1EUdKyj0%2BtXSTIXhzXbTRSNM%2B9Q%2FSBna8RjGbEASbSFPNafVQXf1b6veyib6AbA6UODUR%2BX43qT4poEXlVu6NSKto43zg3fWi7SOPGThmonmhd%2FlSmv6Z5%2FX%2F5scKUDoPTFw9HkQseUuUElnG4ja%2FhLEaR1R%2B3SQCy%2FAoNX5Vz1gkXMw4JHeyAY6pgFGXoLBEjNt%2BmThY%2BzTW3YRB1hvBUL4A%2F6iJEL5AifBKEkOIASTOhas0uQXJ3Rz8pGumVE8eRjahSxf4NJweuiL5OD6BwR6JGI%2FWg7ytWq8ARxLjN8RwKSCTK2Gio75iNHi5D6OZwq%2FyKjZHASSxZinwNDJcTMz5Gh0ytNSYgm1Umo2vuw%2Bo85Ju0QUtyeHVrqHryIp%2Bh318ulVNEqPq77HJYty6rzJ&X-Amz-Signature=5d165e9dccf5fc247bcb30094f1fa5ab1488efefc22458835f82ce022434d880&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


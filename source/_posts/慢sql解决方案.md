---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RO3VNHZB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcwTa0ksD3BikxjF2Bu2s9pLyNSkUo%2BMHXQphScdPL6wIgUWtnWL0GYhOdtRiQ6zGiFOi2XKvz6A4MfkD4iQv8nvUq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBl8MRJzBQhh8s2gqircA3A8ybI9JzpDV07uBeFiX2x%2BqOML8zI2o9Q9Mz50VnncxWA7pkGt6xqT2MJOsGRwS5bb3FdK9Zf8lRAi7hL2PSH3w7iGyQxO7TFxxVgaPiNZddRpedos897dy2sQk%2Bm9Bvo4eaHm5d54DIKuJf5SnxYwhqAo0wb40jhFcAxVdWUwW8aB0OBkl%2F%2Fvgzr0HSSKmKJxEzhp2r3G2kIPJIhVhVWcEdFqncuCw7%2Fqn2JIahdaUL4f76f02f8ISDljPARRhv3wVzXwlmJQtCScDxZk0UPFKwadwjFWK8Z9yry3Q3ff78QVi6VkcDOA5Y7%2BtOA8x1euKQ%2B7a5lRhdO6R3pSaFL0xC53USIGO8REzSluVsQHKkxnJS4vbQLuBjXN%2Fd%2BLTfEus4NLfHAhLHPWhxr2bkdjeNcp7HvlSbQpf4XLhi5NL9%2F75GJlCfP0vQaxUUJr6B21ETxH04UYdIW%2BHSviWvYdrjGJ8MCfWiSaJ%2BPrvEw6Wtrn%2BmcxP1j5H9mfA5Jz56YFZXxURAcu9p97%2FOu9wctmXPV2UAjFJYlwqWWgtA21Dc9SapF6V73jUtuc6sltROVPePA7UB2kEl5Vmmro3O5TT%2BycsveVcKLVqqLEU8t2iKKceOvU96FR1eMoMPK%2B98YGOqUB6cxFCDe9EJFHyHBoshEOB19qPi8jhy%2B7S%2Fhf7TTGW6D0NrJcZehrJajKB8VgIQrevY7RQu0%2Bnaetuajhv158H%2BKrDBITPO3Pk0K0C%2FrnOyZWugb7Pg4%2BvLSKwbamu%2BH36gXKbHcGOMKirylUfXhTKOX%2B80On7du7Yf7mvgwX0xdUxWOGSSiSEf5V7xMUjKhHP%2FqkYw7a%2BdcTnO4pD3KQC1rwDMLU&X-Amz-Signature=b81793e181df83faac664e126b92789267010a13e5a8cff60c59c6e789312049&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


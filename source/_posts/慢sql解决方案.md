---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7YEIDIQ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCIE99EJtcXpM3shgJeHjnNEGCPPp5B%2BFEjZFR8DmlxylgAiA%2Byw5L4q0yRaqv%2FGY0Y%2BiQXvqu3pH2ov2Aj1HptXoUXyqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0CuOAyAS6QjQp%2BoWKtwDLQ1%2F6Mkg9bpz4sT%2B3h6h8Q58T%2BSxJn72LN9KGxhGu0r7m7YeVTmhL09F7NTCo9xtuLKnVvYwv4O3xZw%2FQOh4ACnXT79vgMJcwQ%2FxSOY89GkehK7JOPp5XIKdgqqWoDIFGhmNXq%2FC%2BmrnEFQm9KC2UUWf7%2FcCk%2FbW6p3ZudzjJgYdoxfmbJnv7CECUFHpAICyqNlbB2Za%2FjGJ3hdUgo%2FaD9%2FmVKD28aYMPTNPbD2j%2BtRA%2Bjcfn7cLOL%2BB%2BJt8Gy5F%2B97x7Mmzl69cEk4rqWmDO4wCHFdhSNdIDsPu9MNj4Rkdk1KWPAnbLOvfwRORjdFE0Ni8Cy1opS32xwqvfA0naRCYabCwNt36zlxLtwOxbwiohLy8kpiHsU2Hx0LCrRFEZhbtEeMRe58iWlzY0n3yrwyX9j6Iuy%2F2ZszT3rhhAw92Rz32v9X3Do4BL4yQL%2FrQJDomUX7lc0K39xfaKcxfzMdnWsZWOK%2FBPu9MUR35dCBrU00Ia5RwdXKm4BRLwMqI0DfsLhNmT%2B7lzTj2cTBNL58UKN5EBmptbFE9Q8lORH5JNlI09lNWxgKhuNeTt2sNu7UW7wRg3oNVoSczlb%2FNc2exjwpLt44YiCQ%2Bs5TK%2Bi%2BpfcTtFQOt%2BnY6GSEwz%2BKnxwY6pgEY4LVB1mCa5TNW5blaD6EX9%2FgbGmP7piPEknJwUjvwu4pjMwbTGBg6fo%2BIN8KaAAaJCGaxyHxwAJyEXcKruRoi4YyB6j%2BqpsPe1iI8RGw4KVPjYQeJfJiMQDaMSwaYfPwcJK5UKo4zXvRieAiahYY6QtyxsDmSpTh0kHcc0Ng7R1TJ2FrY0O7bmkkgAtsUwqYi0u33Rw8hYJKSWBiTHStbG%2B%2FOaxAs&X-Amz-Signature=5055e13c8c179a256add479e1ed9597b8a27f038fb6cc03b082e28f578629646&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


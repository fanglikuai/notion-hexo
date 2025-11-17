---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y34O74NI%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEiyGxNvHIwtoXKqvjqKyKz9nj8Z7e6ymB7w7EvBGddKAiBQi6UJ4gVQ4cG4NF2k6T2c72l37ot8NbI%2BW1tk1d%2BA2iqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZZVWLrBAThKiGw80KtwDvpbIIhY%2BQz6Me3Hwglv9lhu2W6tuMQ%2BCXxXEHXkooOwK2I%2F%2Bv8BylS%2BZTGNO8zRpMGaob62vhUSTZR0iaK90C7CfUZ502QEvsmgTz%2FoPn6y4%2B8B1bkrrqIJDDMHviVVOUSbgStpB6o%2Fpa%2BoqLBjFDih8h4mYD%2FRSdbYOBWrCQAgrUbQ4TM08T7eLqBiHrRrrW6jG%2FEYqYSaHWduaZRNXubkc2jl1j2c2VLbcQlGGiIYFVUMPtP%2BtO6vDRyCjFwG%2FJdBalFApO8H9Y4AhH632kJtVeJcTYAQDkEvnYJLaya2aYWP%2Bi%2B5FUd%2Bm4f%2FA4k6Hoyu9TPgreonqzhqeHZtF%2BkkcJZtyPMkMOF7iZmZ3tg3aM%2BllJQW8ntsGql6T52q8qZGBOz%2FdwXm0jXqc%2FYLt8dfRsGHI3OBFCqe5XIjwiB7%2BA7aFc23UpKYKxmCkqrzNXgEXhDVz6mvDu1b1fnnuK6X%2BI7NYDcgpAPAN8a4iXehMQlwdf1ecNDYWdiBUKtWyXojv9o0yCnWjPRYDjAJDiuA5bCIXKhQloMNEUCE8Zl1rQkDbyEAJhPYOcEjJEsBG9nSc9V7TUzfN7GFn%2FQOv%2BffL9VYpbCuglzX2%2FMn%2BpLI9owBb25xUTccvUgkw3szpyAY6pgHpO53PPHTSKyB9d1V0opYnMC31ICaq8GeBx36i5%2F8jBy0r0tA%2F7XIq8xguAKiSNpaeHJJlaiOOhj5n5a0TMeRQ93s7ygk3G2u5PpenzFuw6OcnBBvEFTD1ZgRKWF2sEnWJ3%2FAqm5%2BV9QtNHhHh%2BJu8Tcic4oS9vUIG1TNfRt4r55%2FuGiKLvwROgO2wNK5TWvRzZ07rXCjW0xmvRqtbyt6NM50y4TUg&X-Amz-Signature=94cabb4d1b9c4c2487a48186e8d3e0790306397fcd462ebc0c71e427cf2d9166&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


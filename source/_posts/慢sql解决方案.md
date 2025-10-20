---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665KMBBJFT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T210049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCICBEsbWb2TJskPtLl7h7I4VksPF1YGUtO2rcXDWueVbTAiAb0EpofZUxzcHwaxYtkGWbxxUwn9vl2rtddmNpU5EabSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMt5OeOmCzyeeXa1RGKtwD0pYbqOuUFApFKBds7cpt%2FbjerDLs67d3jU1SKCwaSV%2FGXJ7Bbeb6%2BCG1cLGxUtGAlWGEUb02bNaKncrK0MGM1ErLZWyW3RYdQTuvSjom4782AxCNzplnwyXHbwsVeJt95fh418CRNPSmOav4sIrJ01Lu34VnhOcnPfNCd%2Br8qj4eQcadJV2ok0o5LPM5D2xClP%2BC2Fbd84kBqyRlu7nuOQkFV8maDu0wm6S9qA9oUeBasPuNh8xtms%2B%2FfGkn1dnHL%2FNmSqKrvQX5VpwhvqaIfyDZfc%2BSTCD%2BaOKOnIuf8ICuOXolIRfkz7JWHaEcGawDjsZdkd%2FNzxF9y7DcTgS%2BcXxH79%2FP%2Bt7H2zxFf2A9UBdS%2FnK53BWO2W7V0ICqtZ1J5QC6NuaVWFCGCbNWYUByIXSp%2B7vD57fhju%2BRP8uo1IZXDQN7s%2Bo40Wivu2wX6yssxPmi%2BlCWp4NaOYIjubE6KMP52uugwGOJq9tTf5fUxvJfrSEFT0Zzgwb9H2ZR3EGFQWzCXLMkZfl%2FuZI8J7fqOVNmpnU3rm2hUXb3R%2F%2FwU%2FEiUtnx3XlQ1yOa9snn%2BeBEDHlWJoOhPY3oP3Bu3YhCuwEg%2FR02dnCOaiSfvaAga4BtQrwWVz5sLAwKJf0whsDaxwY6pgHmebuWGr2PiGh6gPpE2iib1IZyz7e1mLfVmCcYgFGL4UXZDnixJo70KivS2NLPk6sTEwakJ6NBE8%2FQP5WxgervXdhNQRrWOPyK%2Ftjr8wrGfS3OP43ixpxD6QXoY%2BbwznXFsqgPwuExtyDVjFJIQykgyBG66TOCtv8cZsdPswlSbM7IhYzDqp%2BFdOfJY4Tg6vHFOly01D4kFfZ9KyuRtk8QeN6GGRKx&X-Amz-Signature=c05bd984acd763680beb6adc8d7b6d6f4db3c9db414b5129fbc6ca384711aab8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


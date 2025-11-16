---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRKQVSGT%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHD9LzrOvtwcHQbZS%2BostJxLiRkTNpf3mIxA8VySOOU7AiEAgP4ywo7a8VTJ0lOwgEYbr2eLeZcfnb4FKbzmozyUeyIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK5TNSUCYhmeEKFkWSrcA%2BpzANLiYp%2B2hMa5SbNHHrn4AaG2PDFdkkDl54E90Cfb5oNaCmwGe4f6HsGsC9kK7KTg9t%2Fif2VA7U8LuSGoqN2xYsVKVZbIOPJ5zERwR3%2F9A01GJpeo1TIO5MxKgHl22WL6G%2FNsiFTdvs5xFBSTEzsKqjITwk2whI%2FkvNIYw3Qr97wqIu28EKF%2B81kOdgTG2BR%2FysVHU8eoGyCq%2BICoqFeNmRGzP3U3BYisVz9QLA1iWKTqZxuFDy5duAIVV8LJciUm5zTBbcSFDQqOcDFcCn2MmQyxexVo%2FOnfdKRE2ldjp8HpR28KARlugmaze37bvOh13Ow0gFtPF%2FOZYBFYC04vRnlNJWUQWPOkm3jZzQdGcMg5LLS00%2BoOanAI1LIsMqkm%2FnWuwj6AD9dJTrhq9HmWXYX0Mb3fSeBMCwHrYtFxOXJpFWAuNBNQtVdORB%2FCMVNK%2FMQdz%2FbYfE9q%2FbUcjZCaxG39%2BZzyaIlHsclex3gvlnJbgk1%2F3hVJoJZFxXWBGosb6c5ZNHC%2BitXwboBvhz1jVbVcWpkWp0dXQPzFYkc9thU%2BnCvVaKxfC2wqHzH21EVUd%2Fc3%2BpEn8fiWOFRa7HVxw5F4gF6UVlFZcaqgFuixqp5dHcZKxAErg52JMMP95cgGOqUBdmSweKo9JGsYjC7dlT9tfMg9F7MUGmRTfPBPAEse21F%2BovEh7zjSUkfmOevKLAD9zeJiYKLFbgCgL9HGGwEmartGe3IomeYq9EIQLH4tcIGuScKzOwQyOQMJ%2FJmzLFKKXjjT07sddjorIgIlE9rMsmmESkuBcvHafAwHkCpNcGl%2F6EyEMsDOAzvo3HUrWylx%2BSDXpOR2%2BpfaYga%2FHufQZv8pdNY8&X-Amz-Signature=3f95cb45b044226f70835eb178b38b3ea7c523d3e9016a70bd044af6aa546899&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQQD6K37%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T160109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIB%2BZ8N8BQGMs%2BD6rUktrJ1ZcYOaJ09SAo%2Bn8%2BSaHW16kAiBXtU4lAQhCvqCvLBK5NEjQoLT2HNq%2BnqwkkqhIf%2BI8dSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1AmCIl47vybo3IzcKtwD1%2B1FdqOsF77DoEtR7jqB86wzvLtOXMyK0IBUwPyZug%2BxPdILEVXg72g7sWbkosYlWskOFFvImwaagTcuZgXfmlqWDwoaY%2BvL0hsI8NRk2leDks9Z%2FHTFJg69OB3Usj19M9DfUZKSy47PbkCvJ4ci2E8l2YIm1CFw0XQFVmg7rgSOqy2W5x0NYAbk%2FKDzt%2FunM4LLmqORjkSt0EA2kqKjg8nBErg%2B6%2FJ1ujHAcBmKgBgMVoydMXK31RqJrz1ypVxoPr6KjUd8YXCdX5Q5TE5Mz%2FyfL3pB5VQiR9UHAK7wtVKH5koE12lXrjStvuwFfWQZyTeD4gp77hxDgIztqxOBg%2FxHaDO%2B1IPKHUUPK0Q7wJEAqTRCRBMvQSbwIczB3Sf%2BhMdCLEuY553xHFFnehEWSCw2kpiwK0GzawIYKLdLj%2BHb%2FhMMEssCQNlxSpFr9GyoIuoC2g1nfX1XNVKwCRl8ochG7ca3QEvcrxLyFNs%2FIUYA9pDWOAKvJowKQ4e7HfJirVbxhJad08CNBizCqIAFji0q51QTHGcXZDyMbTVsFaox52T7Owx2vdIhfKBZ4Y9LV0h%2BraWpjC8hv3tFByFXGpffI98P0T7ldne%2BEbBSEF8QsuHawSiMTZp5Y7Iw1dqkxwY6pgFopKtqdYeYe0ybe07qxQede%2BnTsr8onVIDvIT3hCeQIGPumrGys%2FNdgBfWLBq68u24yhpUPmPZE4EBM8DryvURuDI%2BHqx2W4PmeEpriMKV%2BddiiWIWZhMogpjPO7wSqKr4Om7mRncMXjYIaMfZrkK%2B6dyXMtYXDZWxiWDwYQewW1mGuSlN3uGSGT48x65zJnb0okYPSavuRLn3voU%2Bri7SLYtkUzrj&X-Amz-Signature=282b5f5f46bd4c60dcd75bd91212319fd589fab77713c731a2fd4487b7fe7f80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


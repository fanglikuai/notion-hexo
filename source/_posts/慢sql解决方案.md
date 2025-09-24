---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3H5H23D%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNbFbekt%2FW5CVxbVEuAvpFu1fsApsVA%2Ft9umrkQ6LdpAIgN2JYD2HGSa0brMzK2VKefNFfV41s4nFIsSNnOa2yNl4q%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDE4remX5Mus9%2FZ09fSrcAyk%2BxIP50v7PtC3%2FyKjYBXE162ix%2FyDYAT7hWSuBBltYJCmHhNYdHwRyErUeLj9h2oSuZgvYaYbO6bc0LDpW0Dv07WrES%2BiJfWB0rUHthIo%2BRjZnqjYrtBDT5MBfnWb9GQ6iuWAgje1gMycUZC%2F36e7ebCxGJpnYUg%2Fm7fNQm3qPajcPVxRa%2Bj%2Bn4z9GEcTjBz2wpGB1Z0RxJiROtHmyEIcsg64R09Hvjf2RPuYZO4xy3xpA%2FlyQcMVKMyQXgoySmrellzRC88TSuD55HbXq21CT4kHHcIUeAJxF0TYpJdcQ6rnfQpaBj1uHI73wJN%2FsUQXR8LcM7y6b4H3c7PlsSzMGVnsY1vz1PESJHHY%2BkMhjmFNIIzg%2FKLE3h5OO%2B%2BXmVqZYynELB%2Bd8ZET6ZGRuxep8eMJIPtMZGdnISrUPiB9RqfogzADo%2FEJCnbxyzX7C9Kp7PoQwa2aCnyrwy0tPasKgiTlQ5RReYGRACC1Gu8hPDeR%2FfwE61ii1oW4T0v%2F3sIiNmH7x8n3MsdOoDih0P%2FTI0plgGyRx3yF0G6xj3ItxZIVt2d96DNcikLruNbFU5dc%2B%2BjVwK2OvyrAcS80CnIOyELGr%2F8hX6jcjqRpXAu2HKqMzFXjAj4UIIDVcMJnSz8YGOqUBLXxiHTJUQ2d1A4KUDFYLDVDw2qiL4LnIssSzNWLO3IMSo9K6XqS%2BhXlnv8I86IaigE8At0oHkeBGp2aOSwPPIB8uxMw%2FvlfIrZ3FAa0%2BUE3A4BSo5FRDVliCXuadfIdJFKT%2Bn43%2BIdzhsHLpW4bZiuHMXrQ%2FA846gw4X9grm1IPN2U1raM3sbCGxMWyacQRfiasm0JwAhIheRLiRbtOEyTjyXH51&X-Amz-Signature=6ac1bb26804dbf3c284ce333025a941c890649d6e39bec6611fae6c3d2bd0249&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


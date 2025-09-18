---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EFDRXO3%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDtMFU1uq3mQRfXLWtVXM6zrGyq%2F4zpiFGv6SHxPKzUTwIhAPZsNHY0YDxmQ4tocSjbeDoVPXWMCIO5Wtpa7HSKiR00KogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYPiJjLCUqbGbgukwq3ANqwoA3Jgt1mHPQsNVicAiPUrhNWsgnARejOPTmVCphVbHiBDx3pAx%2FpN4aXrzU7yTQBiDa0n5vtVl8PhYYgiz%2B4ykN%2Bbq3iPeWOWBpjQ0jqdT6GhL0%2Fv%2BMigpuCzsxktgYTMqypBeM8YU%2Fxru8YHw0o6cPjLUWtuFTOsI5Srbp9PFg3RXmxDWHsOYz4yZrFbtqBC20ammaUdWleiX3x%2FfrY0vP%2BZxR8h%2FiNd1%2F2nAyiCpGxvXMVNFoSVVhXJOc86nAT%2FUs%2FGOy75ENtQ%2F4fG%2FcgHEcbEKotrus%2FigudpK4p6Tr%2FAarZxBemUnzryfUNMYuFdcyZmC%2BTXocP9GCpxe7BscwB8E4Akbsv2we2%2BS8PU6NlO%2B8UkXoEfxR%2Bn0sQTltM%2FnwWjgZm9wjOCPQP6VxlIgp5g8eMnRJNNRuBOa1yF%2FA4N%2Fld8FJvhhzaLdFUpRYQvArxENv8zGMTjY6XtfJN2MNKMwGOH8EpQwtNXDaMqmy%2FaeBGq5oFO8e5x1Z2PGnnwZNOxO%2BbjG3J4g%2BR6pgxwxxFcQPHev7X%2BlXuV1PMiixh%2B%2BYNAvnQHYYcnhw0aPpx6Bx2XLTyhY4AESxVwJUt7Jpjfzt8TyhzedArbuBccX17pzE0oMdm5T7cjCgua7GBjqkARg6iXyDskH49vFYDHa%2B9Qc9N98NZlqjqwgp%2Fr%2BinQsWQt3QyECPNJV3MsQHFFwakzk4izrd7djBE8spe%2BiX%2FDgCn8fVLQUONWh%2BwRLM8okuFKPDNzhzrzkarFpVGSnJw1l%2BfdYZa2O0ehYSwB%2F9aOs%2BLwzdwUbrdQDT5Ee%2F89xstwrSAt79F6aty2cHAWgQ1ExI%2F3pPtP2zzzK0KNZc4C3SwaLf&X-Amz-Signature=52591e59fa51c736255e4ea9fa6903dccb776f0acb976cea68a9f82f3f9586ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


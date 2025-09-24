---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664C25V23L%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDLySIwcxum8w056%2Fq55OMV%2FwbDkxawzAhcG0JA7hIakQIgROPVBeREInQ5UWTSs40PEJR71vpGMpNgw3DNfBabb7cq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDEW0iUTDlN6ijlhxCyrcAzykKFvNKl2zCRMYRwhxsNFHkBtzR028lhZS%2FZMS7oyczOERuxq5A1IV7Zg%2B%2F7Rpvc688SIYrFQNqA93tRqOj0D%2BS3IMfdGE0iOExzTTClGXAS5INUdqiSq9Lj%2BslvL5koFa%2BTbtc7zVVKXS5YAX3gzZVZyKMBY2NoZd7Dm9YKFv%2BYaVqmJzDE5k3BcOs2buDKpyhjqEZcrmcQoWGUNP0HrIqafOERUU33pxilDWNwZwyDxx8oKn3WMRYp0l1dCyID8wVvxyl24v7m9kS7%2FCD7Yd8b44QgXM5eyBo5a0mYbe7GdVC2UbdMgDa6l7Qtz2pi72ngVoc%2B0XljOz9fN54TjpR90mJZgahBgBYAbLH%2BeqK2nM3HIi4xiLx%2FaqFTrWz6Dn3J3CfJdyVyVyOVsCOXtzXso9xt5J0orvlkq5j5K8vXJvSV4j%2BvkA32h%2BLmvVGUihyu9kPzbPogSFaMSHwcxNwW59%2ByVxekpzMx31SzQNJcRqQSCr8SlZkQ2gfugnCIqkEazYz1ea68nJw3%2FLYiI%2B2GD6kewyxeqgS6nCqRhOj6v%2BzJ1l2B59c1JNlNZ5JHxMzE9RocPCNouczS%2BguVfei9qDMYJ%2BlbZLCzdLRtdnTQHHmYL%2Fi7YkYviPMJvSz8YGOqUBRgWmLzNbxcaWh%2Fu1obMhez6hVZrmxIc%2Bd0EFyZbuAr0umTCCDjBaqwTwfd%2ByEvuX8tACNRLJN1E60PH6uOngSfDCH3C2biIdyyWGtGwnZUPPPlUX37XdUZNqYXvPFSKTAJdLEwaW6wOd0%2FzPzSw674SOe%2FNTM3bYkLMAyYBm1c46EaiZksHTPVZQKJ4OcEVu1B9TewfjW1Yc4Tr7LFn30RFXL5ot&X-Amz-Signature=5fd259678f37c0f6f9cd0c7fca022371d832242c40f72b8befe74c5f2980ce83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


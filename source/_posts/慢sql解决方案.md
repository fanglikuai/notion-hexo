---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZ6GNMHS%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcwlOAcG%2B3tKoYpfC24m5ghdAqzRIh%2BDNQA%2Bt4%2Bf65uAiEAiUZtRcqzzJzLHo7sq2JSj9cB4XruLfjt%2FUa3eKSuxScqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHXIwuJ1X9mx2b1OsCrcA08CYn3QmkvwcxVGG8aTKpHA2dvPW35anIeNvMPH%2FIEc5aNTMhxpeH51ziGn8Eo%2FM9q2JlttbRyD%2BxwxA%2BsPi8J6ulS1M9F7FnXKcRh%2Fa7P%2FndkrUBwRbx7eT2iNhnJ4TTXNMty50ha8GO3GyIA916QqBJiUnBwXlFaxqV%2BUuQ4FBjk5UJ2nwdFXhC%2BbXbC4l3YV0UkUTmdKvgKs3km5b0fox88gnpSFTD%2FgP0QkQUYJMygycCgaOHZbpGEnsPI6GqTcqZXAfC9xn1kZwepGrfJOaz9TxCEazGx%2B3IeOj9Yt%2B%2BG8ECThaw74XcjDOD1WH5dzBUB67P5WULX%2BrVGjpXxV7TlSZpYjU4UyoPiIpJ2%2Fcrz17s4h3DjwXNFdFOu1hNMTHOHxik0mzQX%2BdDaDLdQB2c4Snmg1Q4EO%2B8XL8n0rnZLTOWQV4vgwlYEzrHw8JTz2GWDPPF%2BalFTQzt2ppMCydT3ZbmK9P0sqbJnOrY%2FO3dWCuDxp0vPk7fHUrksQs89ZhlQk5em8cu%2FGHnRFmpiR3SZWx2MdNeNHfnFdhxrERtYvOSphA8XWi2gGz9vHDHT6hxDcNLh2iJPVP1X7QZKIQaMMit%2FJbk1CSJLn2XI8P%2BYSaxzUGZZiylwlMNO9pMkGOqUBAbH6NfrxE47yMlGT2JOL1wrcNX%2FytoOz%2FXYHV9asol5OA6TqtAU7yvuyx%2B2lPBkHUb4H%2BWXEGV4c1lCJEKHK0Es7AYyS%2Bn2AVqmIzJ%2Bv2qzIU8TgOVUxeLLm0JNeWwSm59C%2FAc4W2mMCjCDDlsggtcMcwql8Q9uZNHPSNm11ea2gKgF8xi6FH%2FEc1uq4uTJdef0YpRSC5k%2BPmCo3ATPxdHFCjC8%2B&X-Amz-Signature=ba8a6b0216f1f42132119cb42e7982bd3a4cd945c51c095af5ac6f31c0a60066&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


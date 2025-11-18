---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662T2DXC6G%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7adOUJvnX2vo0RI62eIsMOxaBJtDOheADcYyAaqofgAIgcFJb3hpVD%2FKTqLFMtATfYfmlOcK64XxAob4kE15yp8MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJrIGe3%2BaLWz0X8UUircA9bR8DokJEBVLNOZD3s09ooNnDjE%2BUfvGQFG1a6uwkvecPrre5GzMXNLA846567ZfuKe0p8nOWmW7S0s5c1wLT2Jwd4p3C4KpmPGk9z%2Bo%2FUo4dsCGMyWQIdtidT82sXmmOAGpiS%2F3aXIoXgfym7uFuKPcl0Ye%2BdlR1B8%2BIJMoljkrOVKoqb7YlphLRCfeKBfcY%2FghoIg%2B83HZcslkHnj5RImmZ4vrQ97h%2B0J5iQ2ao%2Fm4Pnlt%2FUpkyzKHIp09CSJFdUW0F8GcOFeieWszLN97OQYYtjRkqIkccvo44lHt1PecA%2FkMhe3U9%2BMBqoQb8bJ8BurlzyAmW6UPfhgG2%2BSjWlgZDjh5rkgwObVt62ko6tRJxIc5w11BRCxgm7PZXGkW10kjR54PXT7Q1ANCzbQGdsqWCHHQEqOaE2B7Zc0M3TpZnbw9r1KX6c%2B4DpQb%2BnzT4BcfVsy6B7vyJun0fjBYY4JEJ1ZHRnPUJNLPbMPuWHTfHJ9o8ehAiPPcgRDjE9ZGQxDS4I3Bu7usN6IkkIN4RiKgSf3RyNzQBjn3PoFSSgy6INmvZeck9CJEnM%2Fhy2XbGz2QBqDxoy5Udgi7cKB9GS1Abh8KsulPVXO8eXnNE6bVfwviTrcQjpBSV7TMK%2Bi78gGOqUBFBGge%2B9feeBrx6JAEgpkwVXdEfpLQ7bc8XARIA8p%2FvQlOu36%2B0njyrOHa6mIQcqJWJ%2BbznMDmMPVNK9B8fThQsmD%2FEVtUH5zdMsvJOxYNZ79eg9bJjlfcQztbL49pESSEHx%2F3AvvAxtV9gagqQHBfo4wl%2B0jSfaNezIFZo89L%2B9cb2rOodZJ3xvpAWaN4cuURkBIvCTTfgD5KQ%2BsiJaXctlYD08e&X-Amz-Signature=a6e913f04cd6ffd72a8d122e1a4c7a32620f5b8d26395a191d66be4349872c63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EGEWTGW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrMp5pIUOG7NhUJVuqSSw%2BtfxzmkVp%2By1sTorJesFzmAIgG3shvmMhFGNp81r%2Fy3DYtQBkwhCECwG1FI3F7acQAxMqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCae8VJn681akvf6dyrcAwweHxfCS9Ngs9ECgzxGmqFVbklvQSg2LB4KsB7cHlcWebARVh4poe7hUuxqVqbiPhAm2Nz%2FGKfUArCMa5ksntxLGtd8NbOJ0bw%2BWQ3pxaqjj2KSWPoK6gdhFjhZJuGUgLKO3yaFDPbflDuGHQ8NoYrMzgT8TzFW04C6w%2By5ktNRRcJg2s9azVtZjID%2FmD%2F6%2FzeRqtcf1675XQo%2FqCCx20yF9NaFKRVXULjxdvRRzqJGZ7gzIT98ygk%2F8ymtV0IV0yFfvMoiUp%2BOp4endbnl81Fsy8pnlCAN77%2Bjh7u5NDjtHiOyNDcMqvdIgjUfDGMG83sx2Bxw7N8NSDM174LOEgSYj4Xe22z4Rr%2B7qWCPs2V%2Ff9NX840uCV8%2Ffmn2ieIYfWN3LKLPO%2FUM9tWEwieTXlCH2zpU0OvGss9fhdOAmaTUzhyeOgoyKGoxXQcJCQqkC6j9%2Be87yabc%2BI8LOVjA2uTpUX%2BS%2B1KSLxqWIWuq4ymwM%2Fg%2FJEuC1vWzdd%2BzvW8qi1NKzPQwzZuW7MKx7p2DLU0gtY3VKHHSR7VFkeCsW0EjtCbrdgoZtpcFErpKLJdHCtvWzVsW6B8ZGtxuCsGhdUdwmJ6jpgtkjw743LtIt1drD%2FScGwrqXD3r0pmQMIDZwscGOqUBjjVzNzhFzW5LNeeITFyoPXQXh4zfGYn%2B%2FeIR%2Bm9QspIQSQBXLsoBxdToTYV2MhEB9O8XpBP7Sgue6PE1RTvPhS6XmgLJb5K8KfW1ectQ7EYqNo0TyWidVT8pT76gSBOivlxjYZsFS8nT7mTXQQ3zqo%2Fji%2FsNeH21EgkCfXcXZKMY%2F2xxCk2l8WnzIHDls8r0qsXl7x0aB3fUlwPp9HLF%2FGi%2BdwhU&X-Amz-Signature=e1a8808448440a4673665706703de61cfe4e1f34ba6a0d591336255f59b14987&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


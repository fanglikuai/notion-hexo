---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CSWSQ42%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCYJfJ9MI8FNZRTga0NPceJ9%2FQcGfbxR10N9KIziPhBaAIhAOadWUhUS0u0dbtfIhjd2c2jVFQwRbwCghXV84Ob%2BlSoKv8DCFYQABoMNjM3NDIzMTgzODA1Igz3l3SglkZU2k%2F3Wn4q3ANcYAAT1rgZeKbJ3moeYYdwb43wzRJFug1f9lEg8gvwUVz%2Fx8SUPV6B1bBs%2Fz4NVvFHbfDFLCXuB475KA0fs0%2FtVntsle331Qp4iUgDEXVjwxPyBkNJ3%2FTQsYiV7qDiQ15Dd%2FhKP6G8dneTWDfEtStYqTks7nRgsWjkPI5L9%2FjiH5Po7TbOAbXrGKesL%2BwX0oLUIbAg4QcB%2BKTfgvl4%2Frvjn7AIOMyxwTFlXx2EQ7nqdcqf4ygkNun5KhSLuHc%2FtqUto2M21cfuQOb3JxfT8JTSTviD92AK2Hkmkh%2FYZGUkPnFuMRtSnuwzTD4hwEa9M058vVfQqDQuTZ%2FThEPvQk%2BE0z7jOiW0CW%2FH4D6r3%2FdsIRzCojtVUsdCLKuMnvU%2Bt11yy%2FNEGo9TSnObHq5LhXFGVQURu311Wn%2BZkYTr8OslLRz4VU%2FaJwbEFTP4v7Ns3EY57HXLSQyjGl84dauwo7fWDGvKRROWzDvuLKPCN97C%2FEM9LxQibB9ERXouk9gPBHEnzackII67I6XJcz43gvL5FBNTCed7X%2BH2J9pcE60NfD6ewCOMptfLGFXzLP9RjbtORqxAoXUlrl2zHcdcExtZ0WrWgPc2NlBtqw4L6N17K2tX6DlUgRzFq8zDJzCajOzHBjqkAUvYROswabIwla%2F6T6SyuHebcR2f3eO8GHDSKuNkvqbmUb%2F%2B9aBm09R30LohjHaHl0PfB0mVyghJbw%2F0SwWTyafan6FwPsnrBtoG4F7I42U0L7Lhc40I78y9epEJts1Av1yno5tTfC6EIBhHSWDa1TbkS9KPw49OwRj6DcVSKw1ICpFFoIaeVB2LJG6v00AyduCdg4%2FkTDqp9xrDY5%2FcFsLpwm7v&X-Amz-Signature=92fd2027b1a1e4b45b71db912cf120ef7a0a509c8015152edc47570828c7b63e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSU4GYTQ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDkIhgdK3gjYlbgG1RtQc%2FZrdrFV%2FUSrUMDV1Ifb%2FMyAwIgfMvrqOQXaTrU5ZTeB1Ge0s%2BTi14tg1KdGKc3n2Q3McYqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFSsoWlFgLrzxMcqUyrcAxGwBbbmMABVt3ixj8VRyLNEWlqLOtadMDogKWOggw5FhYuxtVud9gTFITJzRcvCsZJz8h4PLraigF9WrDPLAGknX13d6z59bDqx1J4AC91kiTiek%2BsSFLnazSQmtTSmpm9b9GJsKdHy9gvsPYY%2Fc2DEkkDFnRkJeBTRSo%2F%2B3Dx8z0b3eP0wz%2F7%2BM3sfCH%2FPvTMUdqY%2Fa9L7nu%2BN%2FoooAlpMp7QzSuG5Qy9bH1nkphFS1KBhjznKW%2B7txiLG0uSWKkhh3xvSWXbiikUEBZK0bM9MJTfox%2BxFKq0OyFYnk5TIIhr0x2MBDbdPMYdAZv1R0ehM3COH4dMDLsNo6Vqje%2Fcl1EcLhPx8W2Pro3BrWJHzkHgNalb6nWO8hZhsNDD0UDRRs1O1L%2FXLARpOaKBzoU0mlD%2B%2FE6mp6MYTSmWvjEPfjSc8lhqgNvMXvYdezy8ZTUHsye%2F1N%2FtRCxLRQjnweFNcuyPZLwF%2F0EiYUXgRLf4ulX34dRYmO%2FCAXoY9fh1woe5%2Ff692%2Btsr5D4Zt4XyYkZFGD%2Fd6Gb3nSkHlCU%2BotLAcWjUAc9TTWXf8KuWGVdzWTdxBwlhyIH3DCSX1HUaBsL6sBN8CKWUXFGALVZa4pFaFjG0gNxs7%2BB2Z%2FaaMJ2cicgGOqUBRTIuUajATqoI7amN7XoqJ7P4qSL1sNqSACB1879VWsNHKXWf%2BnBpL9hXlTo4ROqPcM%2BhPRIUBlivSzH6opw33CsqPWF2ewqDs2Ql6QcwIU9bAJL8B1HpXRUDPJdmYP8CVXzWZWr2b8Q5SVYe6kZ0V1n7Ts6OKCS67eLl9Rs%2FcrbQFBA%2BBBZ4dieo2558cUGxEXsGhx5diUA2WpubPKL2Uh8Uky6x&X-Amz-Signature=bf85d94ab2bb0dbd053bdd5d52be3d55417bf04e5115f2d553937dfac48ebe07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


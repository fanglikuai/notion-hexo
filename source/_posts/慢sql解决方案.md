---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFNNDZTF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDkqp1Pu%2BwW7765cdoLpx%2B9SsdAQtPEdtpuwa6sNaPIqAiEA845vFdr%2FFiiiMdupZhh5nosGo%2F0l0U15OGOqIBcAlgYq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDBgwWNGTx3hothQF%2FCrcA0ykWPN0nb7EFcA0KNU3hOsJJpn552x6Vx84zN6fIDdhHhwwFS4B4yU04TmlXE6HmsIdEs%2BsSMSDx6Z87A1MTKjKEMWtsZmvkNnCC0fzEwT18KZvSmWDZbkUtpuIkAnQN6pCn08S3MXjGoJN9S9tZ%2BulVPzPmKplnPQ7GoVuImTffGq4T%2FZ2yoCeB0tWccIEmmNwUROuNBBDpzgyf7m0cLNkOokXRZu5649cgn7V8jGrTHY6q36ot85BwHtMnUq2sSuGlK3buNp2%2BYWmNqdV8rz%2Bu5vJycifFHQuS%2BV5jlJbY2hvNZe7WgOcYQNddXjh7sOxBfl64jHG6gEW1lY67UjnscHlucJd%2FrvvibLjPOSbaOXOmOmzAuQqLQUB0hK3O7ow4pi93HEpIZpBgwIv2exkYCP3m%2FkuBwiBGDstV95OyDEq2Egzg9SBprZvQoIVlKhB0y5IvkDfHsikF7i7A5g2OC2fojYDIy5RKDyiIezd6RGth7vajrnxCi%2BdmqLDNvqgyrGqigXXkQ%2BV2d5bWMx8TbfJ%2BZVnQdwAHhQiyyNUFG7AzFC%2F40%2BEvNpcvuW%2FG0lDZl4Wj%2BxeeaYjyril52HK2W3DrVDOgcERxkUnSrkQ65a8fdMfogUiIuUKMI3jlskGOqUBRm%2FDhDXsevevbZy%2FXZ%2Bp9cNRhccaa%2FomKXJmajxa1l0vndLrmdt4yw7K%2FSD3WatG2UU2AZURyZ5ZsKuJ6IyDX3IghUqd7w%2BXYZZGFOkD72COuwvXeY7mn9MGnegJyymRazZWqz5KngAlCEzshuljajbf4O%2Flo207jQyhRnd7TOy4WWkkruKxDhNiWs%2FDclllR8Y6GmIC6xEByakoKbXQoLri7Pmo&X-Amz-Signature=1f1eec14afe2fcf63eeeee17e6b0c391d7ff393fd926a054c4878f8c63b67c4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


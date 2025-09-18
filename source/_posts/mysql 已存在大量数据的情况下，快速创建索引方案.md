---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZRHFFM%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJIMEYCIQCydzWoUM%2F99mpKrbfbR5LlaiDp1kVAOpODQNgN1sPpYAIhALCYV2BYoRt4IvrUrDPJTKBYh9SToc15pdkoxqz%2BjW81KogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyn4ZQ2rIuRG%2Futt6Aq3AOvYMcbMZquXuF8VNA4weSzaaahywVc48quMQpE7L72bp843bY%2FYU37Sc%2F6ONjApYTgvTv19%2FMlaWswBpSbqaAXWa3AlKkoG7L6S3ZW3Pmn%2F%2FKe3CpnK892tUx9aQV%2FjtPhSqBtutI%2F5%2BnKCI8AO1XeqKUSVTNYXIaHjcXzsS1bZMjB%2FxnMDQTM%2FUMLoobhnv3ZqaLQaAkHvUbDbi25dRGiOX59IvkUtlvJz%2BZszkNc2L14i%2FTceqP3SizeIaTaXTarEHlBWmj8Y1Fd%2BydTkmBVDQcEKd19My99Cjomfo%2F0INB7SIF1iGmisPDQu29yPoyCBz4mbrfC%2FGTE8HiEIj74bUrbijjJ3Ce8J8UsvLG2a0wkzELa7QyHUHA%2FsQzsYIfIBeReoxlq63vkoKyXINyHWfaBV3NYJjuy5JQvbAkxDlkroD4kG81L8bzJPT599x5L16UdlTRej6UMxvP9v0rD0IGuMH18IiOUJe4eB3AC4fCvgpjZZ6jkPJiG4kD%2Bh27QVpwLe6PnpTpNhUAFmZW4Bg%2F2%2Fde1vxBfYrUw%2BXgdLL1CaXJnDJe%2Fg04HS7awlsAeXufi%2BIWC6uRqQfbSgxpBefr00bv1M5r82%2FoJy%2FrUncqIG7tCPL4iK4zBdjCR9qzGBjqkASfZD8RG9Wf8L%2FUvI6%2BQXRVUnEa76siyNlBMBfk6fA1FBYrwDzEScJ%2B5OvrYHlfpSt7oihr47luXX66toYM3ziijZ64eUClZxP7fX83wsZHdfL04WJbW4hTS6Tvu6LxhCDXLxJBeh%2BUMNF6lf7119GiUVSSCm5F1olHu9L4fM0KMKpyhf1MOCiU3j4NNAbwXU306EjRZdp02yXrd1UhcS3oFIze9&X-Amz-Signature=d63b76ec613ed5e62f103c7b1d13abc2c118d8c739458b0a4c2cfe6ff03fed70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


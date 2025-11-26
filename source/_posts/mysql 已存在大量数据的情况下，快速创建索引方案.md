---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM7FVJI3%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhSOJWSW%2BWMvDtpQrusplmv1F0NFFygmad1zrnGlWbfAiA8nWP5n115MHRnjiwkTjUXizngKVxFMfATUvzGO3dVdCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYux0JB0F%2BsJk%2BZSiKtwDkQK53byeklAjFf4GCSJ3xk5fXKP2vviEP01Ork86Ula3uydfcc%2FbrFNvXdjP4bbcPXHKdi5x4Rw6R8rfykNVv3Wv4%2Fl2vvyEa7vfUOOXpyWFQ2YjSYPn8nzdauptIVTpJXhEoHk%2F0Vibs8gUZ1tyA%2BcqTpwXUX2Qu%2FpvMNbLqIh7Y2OU89jgbkmH8BVV4ux%2BFYyjtatp%2BFW3pOcmG901EeTPiHUrGr1nCaFlP%2Fd7lXk15fskAAwd2zBeIS1abaBwZGOCxcdyXthYEZaMwOGvq%2FGLfGfkYXHsHKZYxUg7krgRnHquiyuU9cBY7U0q5T0Ko3v1k8%2FOVD9rv5rbhxX%2BDltfpaiD5ezhUW9TqcAvv2OOBvVmKdXrG7YSep8Zem8ZIv53mWFaNGoV75G09WhH1qPzEgZrvfhHcmoilKbbz26ifMHhHvIkSghYH06gvAUfBgauq35Ol3kH4K8S8jhBdrYgoSf7XfIRN4AWzPJSVV6wfGhch3GqkXAWxuv6ZSG06GgGuKkW4XEHo5WCTpkjJmJ1rtMyuL%2F0Bbew5q%2B4Ep5I7M13FXiEEVCkTICWF3uZhJQd6XWV%2FnaF2HR1pbLtiYk%2Bq%2FmspezgF%2B3wbteoTXOwThkL%2BhnN%2BEYYggAwy4qcyQY6pgEK%2B6peOfXlzbJ0cV55tLTC2%2FbbBgB4C8kTqEz%2Fx9XxKKlIr0qfSOZ5%2FQERzou2iwUp%2F%2B27FjiRiaf%2BgVJ5UznqiJjyLqS4PhEtVEcEsuUNJSOvXziP2O23O1oZ4cXFJWCcfpu2TzzoGlglBRr8znf2k9z9O37%2F7jTBqV378LS%2BFHZV%2FZlgQuVdzitz2DPw2zMzM0ZVGI3zShqw4qeHUuQZkhhTyF41&X-Amz-Signature=aaf8897888cd8131272846aef591362da8d413af7c6cac36acc80306c044f526&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


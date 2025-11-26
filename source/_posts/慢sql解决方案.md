---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM7FVJI3%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhSOJWSW%2BWMvDtpQrusplmv1F0NFFygmad1zrnGlWbfAiA8nWP5n115MHRnjiwkTjUXizngKVxFMfATUvzGO3dVdCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYux0JB0F%2BsJk%2BZSiKtwDkQK53byeklAjFf4GCSJ3xk5fXKP2vviEP01Ork86Ula3uydfcc%2FbrFNvXdjP4bbcPXHKdi5x4Rw6R8rfykNVv3Wv4%2Fl2vvyEa7vfUOOXpyWFQ2YjSYPn8nzdauptIVTpJXhEoHk%2F0Vibs8gUZ1tyA%2BcqTpwXUX2Qu%2FpvMNbLqIh7Y2OU89jgbkmH8BVV4ux%2BFYyjtatp%2BFW3pOcmG901EeTPiHUrGr1nCaFlP%2Fd7lXk15fskAAwd2zBeIS1abaBwZGOCxcdyXthYEZaMwOGvq%2FGLfGfkYXHsHKZYxUg7krgRnHquiyuU9cBY7U0q5T0Ko3v1k8%2FOVD9rv5rbhxX%2BDltfpaiD5ezhUW9TqcAvv2OOBvVmKdXrG7YSep8Zem8ZIv53mWFaNGoV75G09WhH1qPzEgZrvfhHcmoilKbbz26ifMHhHvIkSghYH06gvAUfBgauq35Ol3kH4K8S8jhBdrYgoSf7XfIRN4AWzPJSVV6wfGhch3GqkXAWxuv6ZSG06GgGuKkW4XEHo5WCTpkjJmJ1rtMyuL%2F0Bbew5q%2B4Ep5I7M13FXiEEVCkTICWF3uZhJQd6XWV%2FnaF2HR1pbLtiYk%2Bq%2FmspezgF%2B3wbteoTXOwThkL%2BhnN%2BEYYggAwy4qcyQY6pgEK%2B6peOfXlzbJ0cV55tLTC2%2FbbBgB4C8kTqEz%2Fx9XxKKlIr0qfSOZ5%2FQERzou2iwUp%2F%2B27FjiRiaf%2BgVJ5UznqiJjyLqS4PhEtVEcEsuUNJSOvXziP2O23O1oZ4cXFJWCcfpu2TzzoGlglBRr8znf2k9z9O37%2F7jTBqV378LS%2BFHZV%2FZlgQuVdzitz2DPw2zMzM0ZVGI3zShqw4qeHUuQZkhhTyF41&X-Amz-Signature=df71f051ff46e03519290bb637e578595657a0eab93d09c16d8b75150f16ad0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


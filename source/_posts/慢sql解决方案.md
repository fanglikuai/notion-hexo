---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQJWRFKS%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDysqupAJB35FtAxrn8ZgDAC6ej8wjvNGGPNl4HlINgmgIgafv6%2FzTC07Zm2fPCyKwxsI1N7XnOd0udMK%2Bbeap%2BNBgq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDCCuBTFxbFysILPS4yrcA1uliV2mxjkrbdfI%2BzQ325sSCEMUeR%2BsW%2FrDt1PZVuEdSE4jUquqBB6hqbUHwJThgPwgUQIOSFaWtjugA8Lx6vc9cZC%2Bi4Ixj24svOMIKjBIMzjpiyIYkHPfZMtPfeQRqq1u4KKIW3p%2FdY1qRNy%2BjriHAWcTWnvZQ4GG%2FyOvoisiE8xxiYRyR6dyzwazOFKtYXKjDWnWbF4PybuQScUwEzgGLU3j0xt5neruJEyFUH3A0P0XRjy0%2B7Pd08dK%2FG3tYXMkEZG2dbKXPNKDUmqo40Ki9hg6nXVPYJEg%2FkgrQHe%2FT0fszAjRA37p%2BZYt0gubIlNB1j9adm6ivWhpGefbkzheWZkN3hpAmasOX8VlN6lgnxdBjHdvP4siylEx9n%2Fm6n3Tpz8sJrUGy8kA1Pl6gIcgB9OyACMWyag9QdwE%2Bsrnqy4ChrnK1kb1brlZyibbuhS3Hc6sYaJi5Mez37t%2BmLHtGxsQyk2x6dKsLa5YSeMuRYdOWLcgzW1M1aLH9Ov22%2BVX1u%2FlOcfm9O85MoAMAdrsjMq%2B7zIBSKF0NDw5E%2F3GvVvwTRFEmBHTHFthMkO%2BSUsx%2FZSOUfiU%2B7pCdoqRAVDOVBawHzbzUw%2B%2FXYHrkHJ6YXF%2F%2B%2FSwXzLpM0pSMMbnzMYGOqUBt4L8oyNfSMkNqCUG55kcYQTkQP2QuADHfEQn%2FFiFTxLMQ6%2FnY%2FgYM1cUT0W%2FNL4e8FPZnCqG27FjrejiGsSdTuLruMtpCqW83Pu8AWK7uB7jcWT9Lj%2BYlv9We%2FHJWtrs6g%2FX9fjsM7Cp6ZSC4lFtIeeLjcgfjHDc%2FwLAsr4W%2FzuB8Bh%2BONIm1SNLbXFDgIyUEe8AOQkmkF%2Fc1XchzyTce8qDZJBP&X-Amz-Signature=d377cde0e2f5183a3013b5f8bafb785651b4ac89aa14ed9deb0fe4be25bb2007&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHCSDPPW%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICIRp5wG9cYlJir1nG6phZDE7H1FdyxD9n8Xuxfg91trAiEA0Yp6bIUKou4wkV7Y88H3keR9g1N1nwadGb2RCkJSmysqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPjw2sQHGlG%2FX%2B1M4CrcAz8HM%2F0HSDRi9ygN7jRaHw%2FCDbiIjZP3ATNvtKupqoaS2rYxAK5%2FS5e%2FGq9T021ks9aYrtd2ZFc3XyvhyY%2FpUkqkztFUD79TYUhJ1TFxHVS9ERAuOKn9DhOr6%2BVO3z0rhPvlZnvAVEezzjCxnldxIgTi0yWTAP2b%2FhIaPnbwbAQE1CMG6Y%2FRYDa0%2B3ZsFY0y1s%2FQAz5P23GrtqiHNPfE5S%2BCEQlhH%2BEhTAAec5xovIFpyO0%2FpDYzhopki4KOM28qSUWHpDChwDG7jEK%2FsIjmfyWRGS7s%2BbNXC%2Fhhyfe0rC6n22ReMZJtDrc6rmgFk%2FXieew0WETJtM1y556g1PRlqdVZb0jKM3sOJfeyEq2PLN24Rei6iW5nZdrC4dVq7dOtsDFX78wWDhmOf%2BN64cLRiGXIf6zKS7KUo%2F9Nsc6Lhde2zPB6dsuI62gUy3DITiF7y5eytzwO4forBVVoasS6KPDqkF0TtyulDhxQH6kUJMthEwnWNAVgAnRcwqW0EQ%2BQCR3ATiKaeZRDs%2Be79aWxOATVOtchNJRu3vblTGSuXk%2Bq7ndfB75YFHwQlCXQHX4IccaQwcVRWUXMNHdPE99cyJEpsgkc44XJSxSdkP%2FGiAQD5tew6KEcbGL%2Fen6RMNXe58gGOqUBA%2FFpLeX6Cq9cxCZ2PxvyJFDpXVSCaGxCAPskS5G1qLTqBrj3Uco87pabQkhaGgLV5jV8TpQYcLNTes3s%2Bsi5p129n%2Fc02hpMnmQHhTKInUg9Iv9G%2Fg4L%2BBOzyb3hzfdr4zOY8eIAlHVWDCZkwyV0pmziDjVgoWBUHUGsnwDzKwqVSt%2BPm8IF00zejb%2FCb8Ul6zdEWhY8J2HwSFPodSHoIreZ7Ydf&X-Amz-Signature=bb3afe857d38c879b5b3e3d9a22c9b20a19c29fea4f75e4611bb6b722a304197&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


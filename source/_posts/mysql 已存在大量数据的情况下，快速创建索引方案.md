---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQC4VN4N%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCpnAAVT6ygu0rPqn%2FCS5b0IvztTd%2BGuEoI63b%2Bz5Xc9gIhALLnhUVhOhbZOkQQscOlKEoAi1mhqowuhCbuxARF%2F1ugKogECLT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgySc7aG8ujihauKkbgq3AOyk3u9XRvNFx36uwHrX00szPlSKez%2F3Qd6WM066PdXnNFehb7sgUvT%2B1%2FR4VMfutR%2BxYjaW4z7ndKacPk58OnBQ4LOEQNc622BW4eESFqcObp3dNnWkGnnaQzI2EUYptMOJljY1JHALyKmW0QsxPz2BVYFPS4tA2V4quaQ8At1e4DBDDRt8jkiZR1uLU4JZAE7Ezq4sMR%2BbkmliMFofV89AylCefaOnI7uW5al7NqPXv7FTs2cFgikHxWaZa39MaFLJfAzt4cABg4WT6lpbyC%2BySs9IK%2BU1CEXIWuuZ%2B8GhyFv0m8afhe5bkvW5FC3NtElaTZsmwqiV9QSk1ylTLpNrvqAimYMyWW8C3q5kOSVsWYz7vBEoiw7TFpWfvTapA1SKctgZk1XQU4mXRXauf2W4G22iWVptgJaN%2B9j%2BrHUh6orG8J8XVIxJp3iygTiUaLgstopCRickWRN1ASCq4CviZKp9Rcyed7fjlVmDj8%2B6X1aCcX6dlrItgYiQCibXMLhzsmAeHF43gENMPOA5LHlkLb0GGBhsThiIGrkwa96NEotss0Xv3lH5TR3MgaURQTNKJ2TGepsf56twDXaf4Ied37%2FW8a7eu9JQymAx6jmWUeeS6MgwYo4hVaCYzCcr5fHBjqkAYtkRXfREsA32%2B5S8%2F4WxVSwIMYmcftHujlfbltk5Owa80TC4CaIcY5Qpn6i96BuHMrQ%2FaD3R3dISyng5mrs1n28QPb5ywPZWfTHO7yOztiSJHiUFnIo1Awyb%2BY%2BstB39LfSgtKQ1hyvo03%2BIRio25TI4mksZPJfL9krk8PXaweGrPlUH5Dji9tkG5LvxL7kKuRSyHWrA0%2FWKWRXxlPGzHHuTYfX&X-Amz-Signature=0edd7a1f0007332b69342a38f69c6f1a636c76d088c9fb0a3f5793c9bb1ab6bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


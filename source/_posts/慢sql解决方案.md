---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFY6JKO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIE6hYri548FmR%2B%2FHCoGBTfAs5cL6cC088pAP%2Bwhi7gTdAiEA%2FbVDU6rDcGRoD6%2Fvsf9p9DL9IDgHQfiTEiqUpZFM9gcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGKysoQBqDBIOHZKhyrcA6hSFsY9UHIod3hq4bCFJOnDZ9LhekF62Cgi958XvkGFXJkQMgbKLCgAOHi0SyKpoLi92CNqYPfzQRyfoBO1TDqhAIu%2Bse8k%2Bg68uy6K7%2BrQjEtdIM8JPobUiLKX90ZRq9r5wvpZFs6LxtR0q6rUnBcB6mYQqig9k8uwp%2BT9ABHYGmTKiFibjAjS8XO15DZjgZDbJ0rCP0Rvkf%2BrkWI2fVDrtVhU7uS8Yy8iwUqWwdKlBpKjBtGXEddH8x8r%2BhZJK7PuvkWJg1820cBnOSIeXXkFK%2BkX2cYR0OhRlP%2FSst62Z86oskPBCOxHvOqLYLbVL%2FB2jbCKnvSTQtf8h%2B9g3PcxHxTxDazwDs6MwpKMuz8iHXxAz1bp1VIJigWt%2B1nSrcu%2F7ubjDSlIwDkPU5ns9%2BRao0WhZQaK6U76WI%2FaXHKAKoPH0JzVpD98Ng4%2FDMvfE%2BrTrrno31vRoKcVEGMKALnyyOJAyPpiA%2Fs49oexaQYLMP8HCjL58hAMnj%2BRr7WyIaejRnRvLxrIf3HMubbUttd4wDsDJXuRO0%2BYszbEXBfZjqzRnw9l73ZvZSN3gVg4mjdSA9XroX37ZKRt6DETqUgYpx9wLXHgK3u4dS5Dbg74Fih%2FmHLaup%2FcxdmqMLbDm8cGOqUBoNbID7edYBPboKd%2FCLupdxUDZepY2q4SZ9R2Acm4SkqD5KhPoolnkE%2BFA7AGdspLWJPeDgkA5dloX9JYIfZy%2B7mL1VlqwygEkecS0M0B5UC0T138l55HMXqFPhW6DgG1WkW1DXT4P0mjuA%2FbEWFcAj1hsDNH4Ov9fE%2B%2Bpd9gZZ4H82u%2Be6WlhpcsMCfzx%2FhUMw6VAOH6TXRRDkxdv7G%2BUhmMQOWq&X-Amz-Signature=fe1a3d5b78e56e133f7ff8a23eaf8b49a5a7c78b88ddc93f8cd020c51a4cbd10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


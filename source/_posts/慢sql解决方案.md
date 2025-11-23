---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLMZLRUF%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCEhsMQ0xbTeN3iHjLvJMs%2BRVBxdhCUvuIRjEr6QwUYawIhANK8VjEUM2wsRR31ygRPfeZFSwMW5oivxnq1NGxVwzQ7Kv8DCDEQABoMNjM3NDIzMTgzODA1Igx4p3k%2BLFg%2Bw6X3yNgq3APLBZoSaAYlwlIYWWR6pBABricNjKO%2BGD3KdHcc1xagi6hegivPE93UhKRM01Yt52VQF6VtE3AEaHzzEgFBBlRM6A3%2Bqle%2F%2FgqS3URVxHWa3OP0W8Gq7Zg%2FwTjCZ%2BOVssB04WiGE644wLvTqX%2FnqGo4%2FKeF75nTFQiGvyX0fp9jukv49FzFM83XC5YG5hI07pd%2FTDr3n5hUKr8hQ5c4SYdKXRXziIZ6LFYyCjVi1K2sQdZsguNXeCg3W%2FTjPMZGDe2HVKZ%2F6yyEoXvW4vpwI5mMlrqNDGh0iIxaFi%2BnDRleLIrUXTbBpqlzYQKROWNt7hLcer6wXgnjfoox5xuIClDBSH40qX4CzeoHAc%2FEWudu5w0kzBhNfXqo5RlHfbP52IqJYf6QGAowE0JOqIeIEFbXbrLNR4O2fd%2F%2Fm%2Fz7VTjp5nJT8fhHpZLrnxIE7UssVRRri1khxa%2BvaqnCyOaoMgr%2F%2BLFu9is1JqxKMcZzBmQ%2Bb92AqdvfWlsaAaHQOKHuu%2BeDkqzuzmReXvF9WoyvMWeMvfKuErEi%2FhpDfGDxoDpdTtkAC6GfzKhpEGpfMKit%2BRr%2FodeFm1Bxz8EwJ9I%2FyN0pIM1ex5lUSenD3NNXZ2kTo5YpdI%2BxQV5GC5VeYTC6n4nJBjqkAaAF7KyaigTdXQxe8S5PREPdV47fZFE8HyRLYUT6PiTPEo3MrifkpJcFK2AjVGCjyDBBAs%2FFl4Mx92rStgfOfKp%2B9xsEvQsTz7XBs%2BuoOTMuSZvSsg9Xtt5o571zMtY0wK8Gd%2FwGO%2F8a3ycBeayidjGINQtPa7xyaLSHALbOVRsjeKlVx2mf4%2FkLI%2FIaj7fbcMnLbicT0CxZRs%2F%2BDw2pv3mwt21C&X-Amz-Signature=3b20c790098d2f53c0c5b2805640be5602a56e7f3871023dd298e7689a194da2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


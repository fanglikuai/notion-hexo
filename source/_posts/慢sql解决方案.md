---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIIC5NXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCDDcOGDLUiViNC1%2BLr1GMCUTxWbN2sTFxx9fI5Dat3WwIhAI8SHORk83VSXnK0WKpMTU%2FfFbiPpEgL0V%2FRytok0%2BGaKv8DCFQQABoMNjM3NDIzMTgzODA1IgwVddjomAn91FO8eXEq3AONk3S1Hf3rPyoVFKQddcvPdegm66UySniu7JwByPR5XNhUc2m6CsaAC%2FH7upz0yZspMjF1cQjvfma%2BM1I5NptHdL7fE5LPUzgj84Fo6BEqt1wPn%2FeR4B451463Sis43ESQhIKxYMIgSAhwYkwPa3CS0fZ7SWpdv%2Fm9dtDE%2BQEq82DqfqXw7vGd%2BiZJYMCZyB5G2mwe0%2FUgTn9qBaE3DwMGyEgeY0kYVHpAeNf4HOS5TNbzaY1LA67wYDJ7fpgRVmUXD5DRObJDdTSgrba4ksPNO3XfEgsHSte9mLPpznvvAivP%2BvZuCKQ7leiVhSZWa6XChly5MEimq%2BTmOTo7XV8pyrMy9gIUrYfORSyf12QcJJpgP64CBB5%2BWEeJFQKmcGi%2B5B0evThjeRQDV5kuE9BZ7l0KHoqj%2B7VryhqBObGUzg2qK1vm5HW%2FI8ALxZxEc1bMMYUxaH%2FiCd4ZXG14AjDwTZXE5xiA4euDIGXXZ83TQLYpSDeFSLFXQiORXazDDHZ8%2FVOFx2mVs0J1QCCb9Mx%2FM7pYr8qCp4OX9JrmGydHAGAxWZ01WnssMu9Ul27o%2BFUb1Yeb2qwlJS2ZRWGmn%2BUCuSKcCS%2BL2qzTSK1TM4azNsYe8WVr%2BbT%2FYRf0oTCT0NjIBjqkAVll3gtRuz676hxqNMZo1s1osL4MpCKEw0DXzKeH%2ByDnxMXVE4%2B2RXvgmjemdBWKfsYj2JdwC9uJ1do30F3kFAkphxu1D2PW7Rg9pabmFag2c8ceAd7wkDS1VVIXsk5eJibc%2FIvOUFOESnLFVuzQ3vFJMDamwKZDr%2FQzMwTzR97NmEZBnStPzTptHmMACsbVcrdC82kQ2C2%2BaR%2Fv9Lm9vshcnzvg&X-Amz-Signature=6c30c1ab8781902dd4f66714c12bec6ca5208c8ab670c5d7815f484650cb77d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6F7ROUX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC1liHvDd8HCs9KqQvqSmFc0Ena2y2NENwQgKtN1bgapQIgeYYtNhWXfAMn%2B3NGoPJElwqWdBF2pG2QnKnIAhk44W4q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDC7dy7348baSzsMkNSrcA7C9u7CkQ02h4zEzAbOG4AS4fVXJMyvSrtZUck4%2BMmjUsKVWPPStPfmV2SOl%2B4CMI85NyROlrAcB1dCuXI6Bo7TU24BIlkVNrzR0FUjNRzYwzCt61xOTgJlBtGPhrpszh7AjRFExF%2BJ32d7q%2Bb6OsOtkiLztqA4S4biLNlndJwQGdMNe%2FqxNoynyW%2FugWx36m%2F7eIV%2BfzexLKGeC537jqX6wv4CvDdktgpsP68xnOFFInm7C5Hxn4o%2B%2B92gBhQbjq1uF1EKbBHg1A3yzqsSaLk1cfgNQbyeb2QvLEsSQ%2Ba%2F%2FlMKRplg5k%2FamGuATJol%2BR3v4pMnkhmhQS%2FDQ0s0ZHJowl%2BySlPsBY5teRApD9F3x%2BKWyVFKoPFMI409VdLgEAE8DQFDJlrVd7ly7Ev7xpoen%2BUSC8%2FEwL%2FBDNzcDBBiWxb7yFgTTpHXUVqELOSwO2D8bwEar%2FAxzjlwDC8F7Balf%2BRjNEZueEmT8ucjE0cvoOP7%2BGkUEp3uD1yENDVbsIsar4reOcbFerga5fSKLS4SBo5lGq%2B9SyCIgQ6tfrFu9hM6u6T58UdW7HNgjp0BUksnF7NDAg8LUMM9hFjtUAghK52tTM2T2pS3tNjHAA4aXsLI1rE%2Fu7W%2FZ6NJHMP%2FXkskGOqUBV%2Ft7olwhvjU%2Fg%2BG8tL0ceKzHsbiBCIQqDzSZgAFKwCCNDtZQnhOtkLsxqyeYQ%2FaGqDk0UkXvdf5bFGzQ88ioA5s6r82OGTg378TLxj6A%2FLOuM1XOYnASpuQYdd9O1UMnGLA0GbgQinSPfAivR5F4dfSDzj8G2n57Uv5ItJXDx2mdgx%2Fi4pkc1EUCFVrvHyXpSYZuMDPC0lgEXLAtf7pXI0el5FW2&X-Amz-Signature=1133775e8d6bed92bf6850cc0ea931ef519539705c21d0065c4d8d4d53ab4724&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


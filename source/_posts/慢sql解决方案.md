---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBXJ64YK%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T160058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2V0GURuJV7XZ6jTADIN9QrYSvqJrD5RZvG2Et%2FYJmbAiEAzbJtO9kJYYrSvy9%2F4mkQoE4MBOUzfgbqkpvi2EJMAIsq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRAfK3zLEX2GUcYvircA9r%2FP3ht4g%2F1P1x619NLZprYucy1a9b0Uz9eYYGZgSeaW1aBdh1BtJKBGfOYyOJVnJ0GettK9cOFmP%2BDN%2Bes70wg3mAdDlKEJhaQbKYVBR%2Be4Qe6G6l%2FZPnhhmH%2BJ0scdm%2BIoPz5C1G2uPbp46LztrS4j0wPaZh2sI%2FU%2BOffNX57VMN25ULvvedMKafqnicBW4XT4H2v2PHiBjN79HK0cu7%2Bb3TYtE3ogtxnAA0YqNhyeE7c1%2F8yKDDS9zXZk%2BXrWcInhMVc1TIYOciGAG3pqN7WqG2ztpuekVm7%2BtfIRjaztZYycRyCQqziKYTGefq3jlgyNGlWGdRSbaeoJSLdAgydLE%2B5QtGaY7UBSoiL9mrl519%2FOCYwrdMREXq5y7fCwkH0o5q8e6wSFIKaPJQmrQ7SHgwTlFENVqh%2BtdoEimh70vj4Gs5edUaVwjkLEDk2nfOHL5z1cSLR1YScNumlb7qbNpmt2xe53y8BC5iE5Y8uAzf2Dxk6Srq6TZiE5DlQSrruzrwKD00ySYtZ7HlTG9aID2EXZDzSJ%2BFC0ivxo0CkiU9KaEbM2FOo%2F3by%2BTecRBHmvxC3kJiyZnn8fdk5W7d0ouYcjP3b5HBWIhZtDzKdsNlAlFpb0pvm1Ju4MJ6Ro8gGOqUBKVEMPDpiRBTvgOz8MTae%2B8xD2h4QB2vY0Y99OBiaIHHCpP4FTWc88m9vuwE6lM%2FYmbYRa1%2B5vYJYPJ%2FlW%2B92asIZ1egik6%2FWkEUxFIt%2B%2F4Qp9%2FmGR0yz13LJeFaWVxh5J6RLggFZZTu30L%2Bu8ARy2uL9MWeO0XQ6SdosQa0ufKR6eLVCoGjZtiatHKUHukSoW5gXBGYT4UDfRzaPntesDB7GwuhV&X-Amz-Signature=c73e29a26d887d3f4741e7aeb93a0e19274a6e415b7b59d24a862c796302af47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


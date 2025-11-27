---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKFDOVZL%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNvMK851TxZCAoHRCiFp0jFJak6g%2FXY3c8MdcgAYvHSAiEAxSjJ6%2BoXd8Ya8W0zMWJn6to1BGE8L3gP4WShqqbLbpkqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPgEnhApzMQ8bVUg1ircA5ApVTX4eLcYTtl9yiGV2lzytRTeLuVB2guybVgPGybPpAvjuDvyfJcRpQAm0P5%2FiylQ4kXQcUrbDpLOHl60RewrlfBLi57Xz90hfl1wo5h03X3ZSks21befhP5YRdzaogxX8UkZKs3F9W4mBDhuNgC%2F1Aj3DfpmnjhYmveQ6KorkUa%2FqZrwcyHJyD9fpvlH0bRy0738pDiTE9ZiEEk3pJfFzXtm%2FcqXscU%2B9c7Ss594%2BfyJFs6tADRMn%2BnHZeAwxbu0ReMWiK0mle3426TRBOKXljNcnvLa6RBMmUwrHbPHdVek%2F7iXnQ8Mpq49lkkAwnVOHf4iby4QT4SetCi%2F8sjWWOKsblE1OMtCq0QjJE0eFheT2nHO8jMEe5sg8%2FB28%2F0%2FhnarLIGachKj1hGe3JP34%2Bg1hzkLsp8A1Kne8sX8hZJ%2FM83hPWCofoQpACCk2YiPN1MdJgRoDy3fJO1z%2BJf80SBE4gLUJhGMzEUfmpAYvXUrOwIMhfEBqWsT5rh%2FzmEbe1hma4FqyTnuQ1yA6o4VzTDGSil7nYRmaI8a70KjoGrPE%2BRjhZ5g%2FNqhgjzNfLqvWAjyQwyloCsqL2J6HGHfeMIdnV40T6C4QHvTMXZsUWIO8nXgrf2Uj2sYML3Zn8kGOqUB%2BHPWEe47LCb9CF60OggtorHqQKHv1ug6%2FN2p17layCfsFt3yBSzzITCOr64qNnZ52SDNMmA3e6SAaHuoc0ljMyIFahMv1qgFnblslHSVOj8SV6IFVydNO3CpAt%2BlGasP8UEVGHK1DRK36ntn00n6ErBg0MHiGKQ5We63PsWIAilqza73WiiVlKLD6fujyE%2FQIu7HIV7fyX2No14OmJuVg9uFXIJe&X-Amz-Signature=9459a011968d97ab09b422bb2b568e1e9e5867e15890934f13a7995c11606504&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXXHR2NJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIFgRhKFlk5Q0gDx4D%2FPPU5%2FSJTkMc9DrItT9fVanR3pXAiEA2BDyTIEinANyqAAcE56shyrP%2BRT4MbNTlDbmIEVfr6EqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwJMf1%2FT0TumRWE6yrcA2I9XoaPuNDypHOFDM4Q4jzEL4TCCDf5XSlWrM%2BX3UdxUrEb%2BrPgJO2%2BYXX1WIeT1h2wJTSjosT%2BbcNlxtv1yMas94CkguWi7hZ6edm%2B9grLLpEP5uyNDcyAK1XIyWS6PeMdMH2wTTWxW0URajllGosXAYr5lGeiYv57oj%2FLLNXo50%2BEiBLcdcUmBUitCdjzRGDT5YfJT17f6f9vooRDr10zYt3sifC1Sv5yEBwq3G%2BFUXfBNF%2BiGqfwN8Gi8jHyqzBB6i3jhMs9ac3oLYHlnQm8eeJhGuKLVZ%2BYJowA7HSmDsZld7EWIGYU5GMjWkEMEnpaIb%2B3Ul04cI%2BbLZoGr1m25lISbzUYuJxFGK4%2B4aniE9k4X8FHbkyFfaq8U4F14EkYbUSNmTNF1LO07COSXO3x4XrgeZuNLYUkD8Ayj69bjN8hDCg2Y2Chhm64MYcR0QRLtxM77Kgh3lyR94co1ZfTeJ0f1lfRqw04WVnQMMCavWC4CbooZp7Aa7%2Birfv5i1G%2FNEsiyttvxE2MfgHbx24UkmvWUoJiXoFJYzXdalJsKwUhMB1J6VGnyjAUX6R5yL2y0Oa%2F2UizIekypyRwta%2FntKCeg3Iij%2Fz0%2Fujx6h5zLU9%2BmSav2rFVrmlNMJua4sYGOqUBgma71E6KstftMR6HxZiix8%2BKScMDgqUQdcusAcbI8dC4yJDt8xie2guHKMIfoIf4sUT2mkVg6SAKitviHqk82eUlt4EW8DrAoy88zJRINy4kA8tvd%2FSaLDlQrcg%2BnIaAbR558S4z9sa3nZF1jjBl48HdtG3L%2BISJvjTkBJqvRoPjQHg225B2LKCBLJWm%2B%2FUjKXynC6z0eH%2Bs%2Fxh4Q%2FEByC9P9I1G&X-Amz-Signature=7062c5bec4c1181179c675b47d1140f3d6a9cda387ca30e9874ae830a4d6a0d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


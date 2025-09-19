---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWATQHQB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCGeX5lf7WYIiclP2PaDT%2FCoUOoQfHY%2F%2BHHjvk7d31p1QIgHEsXtm%2BBm2HLJ%2FfPtlL086CTtkPU5wgWeneApA2Zo3wqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEAcasX%2Ftnc4OPExQCrcA8anrEcM9imVEZUpFk7AcDD70xEwdqSDNCzqiaV%2BwcxsZO5Ul2Exx7saPrlQhM4maQNe6tIDSiloBLOcMUIF0dOsgG2pFEGuEic5T4bfaJZZyhdO1E0kt7pMQKFKSUlU81rghJTaP8I3cHRNbG%2BM9AVFqAi%2FCBBC9ulE13I3oJdSyhlQ1fBPfAwLzfM617dtEYiNhcUQO7k39eTpdUUEQM%2F8bHeT4o975KxvXM4%2B3D3m29z79IXoPEBlP4KNxLgFQxIdkx3LNZ3LOW5qb6CaVQszD%2F5iXl2rmVB1f%2B5SEcsJiWN%2BQvJsHNg%2B4XTsXmZwKr2GfKa8ps4Frc4%2FwlpDWcHfakgnPJpGP160pxdFaJUUCDD0T9ZVTVfbbx1w6Jy%2BUnZWf2UXWPiasS3rvf2BsoIUY9AOuM1DIFuwFzYGBS0kEWVHD1u2kzoym2461YeML3xJjCuLLBUQuMXvu%2Fibx9zU4mK6GRpRUnDBnv%2F5p377bk%2BAfBn2262hqVcCvjDeTzCDQg3ZrgSynqurCY%2FI3FpmjhuYSdYKBq7CsjQpoHJZrVMcWa3qe%2Bz%2FvoV0Y4yEScLmTDHevt%2FWj3%2FU4NKzqQdWkPLGn78wygTWSpI%2BP8dYOeAUJFk2EE97fYh1MK%2B9s8YGOqUBK7c%2BSP2j71v0Wz0l%2Bka1P%2FhcYWUz7hHpw1SBm1o2d%2Ftb8ixpYT8r%2BCpAjd7JxszwvBmsRcC871U2kwh%2B9T2N7pS0YU5TcmQQvfwsp1Kg5FR4YUn34NZ1XHc6PNiMJpKC4eGpIxEFJ3APOSLPu7Mqdyd9HO5semT969Y0KVm6jk%2BfX%2FznQUFsD41ooYo%2BKJOch%2FY5CDkGaP%2BWTli%2BhQ2OiVKGADp5&X-Amz-Signature=bee04a4a0a201be8a832a49a9f15ec0ee644c66a174138f3927c4b5310a9f991&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


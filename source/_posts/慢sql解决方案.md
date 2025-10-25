---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466273ZX6ZX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBroSX15VoP2IQLTOL6PzziDO3HQnEwSlbE2lfkdjVpJAiEApUeFZR%2BVuFuSuObxkArreILjWVQNQo4R%2FenH9jSLTWcq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDE0lTOZ5g5ho5Fwd9yrcA%2FnykHaB6u%2F3%2B%2Bcs3Fyt8yeazukZT8oIU2dmrxJ9Yf3ByuTMu8YGmZV%2BxJgcWIOKFtZ07eV%2BOhspSLdLQ4Bzs8AG7K4VT4kxBYgasF%2Bf%2FLLvKmuoLj2QiNWJBBcoRBIzfr3q4sGQDa3ZeMrNg3iIdlPZuOOlEw7XM%2FRR8YKqcjVJKtkAxBhB46i3QVL8DcLGbIbbMZNmOu6dYgh72MpOl8OY44WCfgmRIVmO1vm%2FgSlNfgthDuJbFWNGU%2B95xg1C5YxxkEEGfN2wbq0RdLiiN6oJZXthemInOu8c1XNbLmXOiilvp9KtnyW12RuU6kIL24PeYEgsiqs0vdnOSEYdtRNXJ%2FRXMRI9%2BF7StPCt4gwVpVZBNPxTcUfckgc1PZrrM6j9dFXS%2BKrQqSPltf8s0%2BFIrUOSdwOg1ncmt%2B5oc9mkI%2Bj577k7%2FTrqP%2BqGF7ENrnRsdTWb0QSedyPxan6TgC5C4vivgEQCTHUSAn0ckfTn5slYtij%2BWG93%2Fpcq1kjxobM%2B08foCoEPSM6zPeyua4Evo3NxFrgMKz96ehRzfcyBx%2FdSGBj%2FQd%2BU30wVzKBcahFLYS6ScDUSxANJahWXmKGUmbt11kfWEp4Vmtg%2B99WaV%2BmXDIA1Q%2FwGvPEdMLmz8McGOqUBu6KCaAjAR%2FCP7ASnU8A0yHI6K2A18YoX5gFZ5Og06KuguSdSu1LmmxKsZSj6oikyBhEqEFfVYIQg0eDqegPTE5q%2BG%2Bbb2YqTWrL7OtuelG4%2BwFrgudWzmsZvnAHI8q7PRpDgRondNlAHpnut766t0TQoC%2FoYulttPaFpH0ut7h%2FXhYZSQSyX507KWtv%2BBESBo9Nnj33DIvPLTOsfnfQsY2gkcKG2&X-Amz-Signature=d803531901f75dc880aadc92010f5a48533f22147faeed349b32189c8cd21bc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


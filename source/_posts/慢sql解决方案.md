---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQG67ABM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBe9do6qoyKN7i7PervhuY47T3oMkNfFERBbUgM0euOtAiAT6PcOu1Kup%2BhYFIh3C%2Bv6xRCjoH%2FC05R71uya6taAbSqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMA3e0On%2BsRWQaTlrpKtwDic1qMa1ri8zUU2NbqiepKtKgyojFFDFxoH0DyH6Yb0JusokdH6%2BGy%2BSqkfxsMpLvCt5OVVB7luUFhHt6WAIHCwuD3a79OvYIpXJEqC69LgiSWLzi4Y0uCtlEwMEUYGWzonZ478qP6QEqyUtlVsL%2FMnLnCeFYepx1Ap%2FNnYNgzRe6%2B2breyyfqSTXOWv%2FPX%2FX6lFU9RJeuyYtMcpaQUqTGgjVqY3y27IjurHD6AviUwohE18O4xwxwtrEg%2BIvWEbOC0P2ANgqGJwS54nV4vNkee9WHcxi5%2BOOlaemTuiCvLBdoRytTDSNHzQl0omr0dWDt8bpX4F7gwrFDmgKZMFvNJYchB3Cw5rpaxDI7NBTvNQYI3ih92YsLLFAbImnaaVCRoqVI2hQix2xfzp%2BUzPZ8S40ugqeztfm6YffoPH%2F9LHW6T%2F%2F07Ef%2FHXJzrU%2B%2FbZOlxKYkcYNT3jKPhVJ3bNSrvtcY6ttJuDILtgh0mXH4FiMnf3C4qsonRc2WiJ8b6OpPxoXsAvQ1Dup8C%2FxKM5wgD6n%2Bws2zsy7XB0bSdHT3MaGkj0fjF1kLCx3RZE6wztO4JJL4kFaKW9B8euXixd%2FbC2%2FTobUwIxrwXjUYQ3N3miwVsHnrhdoueJ1KK8w4JTXxgY6pgGZq%2FAT8TGTbNI1bzFMuRw4wOQxpyBhO7Px%2BowX%2Fsyv6X8Tfd8Z3kZlxyd26vvELKhAiTUShyNOneno7cNv6MD7V1hJ3I1YlYwnozJoIpiPm4gZMnsZfpPMan8tH768onvPNWyLUr%2BMa9Vp5ip%2BX%2B0lCDcUrldIPAfIC2ebFGWzmi6fwekZ6VdHvDaY%2Bn1%2BYvZfUsJ4bcZyiYJLZ8rGe748cmxMOwCM&X-Amz-Signature=5643e2fc18216273e2ecd154f0c30488a10eef249447f6fbb96ef162cb47bf48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


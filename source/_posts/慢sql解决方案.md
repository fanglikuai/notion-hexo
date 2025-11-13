---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEBHQYJD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDfvMR5C0BvS5U0r4KL18UHYkLo67sDOAESSLDWCBiDNgIhAPJ%2Bmddg9gSIP4G3RYy8XZ9c%2BCzPq3v8fbnjrq%2BVPO2PKv8DCE8QABoMNjM3NDIzMTgzODA1IgycMiiFDDjJ9QwD3%2FEq3APxCbe6bsDVCotr8a%2FbNUqm%2BDByKeP%2BHYCtrqVn2GBmYyqv2M0Siza08KYtxYSD2cTuiR7kn%2BgO4eKUvgP4Z318WdAvOuRxFfPYmnttXRWKtBUmQQtahwVYEdX4LogV6zJmkdjhyxU5rj%2FyqN4ywbVV3aWpZ6zmhv0YptFTevkgYL4tRU0oUlFOO4y5oTI%2BNXRTVr6LPAtYp%2FDSE1i%2BdRsc5PLWTrUAa7yAxJ2mzOueYb382Gv2b%2Fia59q87rQF3GDSQr0a%2Bp1ABIKpOQ2cxe3OG5GRdZxm6Ulp5LJXJd%2F2AnT8UTdidE6a%2BKwY50CeTq0VuBbNpE9nNqyJbxYSwMhucSVig0HgJjgNJpN8ICLeXQd7fv2d5F3OhpPpcaJGat68daBgBaqdPtzvVsBeSIeiNGSn77rsDc9LzTCRX13%2F7ExGnk7PsXwVFDPTJzM1NZrVWRCRecR1263ro3cD70QkmrJ1QSm1rTQCi3Jr8eZ741XMEhmR4lX%2FMDx7iS65kZLaJ%2BxqOIDIXLtdNX%2FIi9Lgy7Id0Ls1PAjndrb1Sdw9jmqOmNzUlhlEOMOXMnWm6%2BC8rP%2FaJDfyiqPU310O137XHJ2Zm%2FkdL25BLTg7DMhbQnkRYMqyVZMCuB%2BjfTD%2FwtfIBjqkASMjUQn%2B6Z4iYTbmisJ%2FFOgDCIOSDTo0OlBHDNGPQG09Hd6Yc2lgdBnMQ0sUHF0WA5MZRyRRytRqtmHgNjkFpVcm4rFvtzGeYJ2wLVHEK%2Bpd3pZjNJg7F8U1f1z%2BwOMmWFtNOP3wYQXxvHRZ2AZFR038pnAKir4IOFzS48IziXNyb6J1MU43ivcame01XpVPqsYXLNhMK9RjJfj4HCDhQRP189ZH&X-Amz-Signature=93f43d3d0a17d25855f9fcb8300a5abb778a92c4ffb77e78a5e69b88e82c7880&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


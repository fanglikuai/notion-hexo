---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IRU6RCC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEYfRW2IE6W5w6drL5Wm7w98%2BOH6qPrNSdEblHU08GEgIgJSKX8IuWVEbZx5ah54p5IShoN04Mu27o3Vk9kEA4XcUq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDIXK5Szpns%2FUDwrtxSrcA9TcyZEe5J6zRDa%2BzDvXFrYlxr6Uw9uKs6Tcdvr68mLJdJJT%2FJBKAE%2BO3Unk4Jpf%2Brp5nn91nfsQlU%2FGnVyZPY%2FFgJwxyb3ElyUhFrR3cl51ULuhMkD6SslGpYJH1tx79AfRIdXc%2BiMSYiJPlbpAydqKab0OP9d71DpEciNzwQw%2BFGIHABFv5iKd%2BZbtU4tlbDEADxbU35m%2FTvC6RbHgh5WGyC1g93T8J9ZKyX4bk9ftbPPSxpAddAPGNFHtOmQg0FJyqAiP8a1tVmIrJ9BoIgr6SyVu9Dm113O83Y9rJS54DD3MLBtpzeZui6mgdECi%2Fmcu9QrtsgwkD5VTejc8xtB3hFRSv5UgXHivCDT7AB7FU2G9tMMw6FVwFXupmAqyuSa6rXhOPyrg%2BqixJnQ13GZbA5%2B0z%2FQO%2FtYkn%2B9BLE%2BOHwRqsMz02Dh%2FbbfQ2ZQhHpB%2BaNjpCn0SAZ4vW3ZinV%2BzUuHrbgInei4gpai2e0LZSkRtFWPO5qEHsZ3z7EOyCduJk71Gz33gRFXPPNE2%2BXwshkKETttLFlBcgUGMVdMJykTeEsRPwADqVA8O3BrRKqcabHCd%2BhR54naeOvZ6nKjXR0x8WX25u8n6oofvtvxYIWPPa41lFsJnyWBoMMKEkMkGOqUBf4BBvHqXe3N1HBFO1Gsc%2FyZnd0wHi6UPDFWKEqBLfma9hW0EtP6ybjQPo13vyQk6uTe41gmQtwjQfxYRqdDO1MBt3QYQSAVaeLsJ3iMzaWFV5vfZW2ehKkCtJjJ%2B65Ww4uz%2FKWZO17Fnxy1kPHIjOna9g7MBC8VInlFzLtovF2F69YUMXMjF0uC1JnsTq%2BwNvgC25QMu1coh0caSq2II%2BCOlvPuD&X-Amz-Signature=821c7561e209552ef51a827ae1b8c3b1361ed1772e0b116fcc79ab844e36762d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


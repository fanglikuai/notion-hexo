---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRQQIWW5%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDwPCedgZSa6LDQyZbdPZ%2FZlnPxaUpspkx%2FIEi2CDwDwAiEA053CziBvsxupxkkWvviwa4dcsqnoFf0EyrUysLT5hCYqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPew0q60n3QCZFSXIyrcA9ljNMIVlgxb9NeC2z3uSfmMYRmAKYiabef94kQZDVDGHL2fMwItLq5%2FzWo%2FYf4%2BudFee9ABNA%2FA8So4LDlsm2ki%2B4PGQ%2F8B8W%2BnmvG2leyHhqUmEdDYqE5Ckj8mX2cghGGj4CGhmmw6vKHJIeoHYzHoH7Mea0sp0pckEqzUNuvyctBfAFqEg4K14JdygHMCJ9q9wwfKf%2BGeSQaiRKRrqniSzBQuh2OCWk5vtPpfWXih1lhMChoWj25GNiQc%2B3dIglF%2FlfpDK89sl2vxgHqxt4LCcylpHoEjs4%2F8odxR3RfKNMvVNl%2BZUmkJnKp4BR4Sk6z7OLhlkl%2BGzHhxU7NwKwXcuesAwxrUQU9t3oIe%2FThHzaGwFhs90TV66TNYjlb%2FC62E3O5pDY5AatpO5mZHo5AzZJlGuHBjVCTw%2FuKZ7fBTqocLpx1SFPLDn7pnxb%2F5saQWfIV%2FzOfqBvNlTjkH1yBC77yk5dTAd4w7tdaNRz78%2FIHlQyu7vgMYLfEKljNfSJGCRShCzwEfD8yGWXz1IDJd5L%2B4mlm79u7fXkbMKT0bUWBByqKz4SIPtD3DyuftcMkdLl%2FHorLCY4YRCLUPQW6GNry8tSOkzWGoZCViHKntObMzB%2Fy60oCZyFNiMMr%2BoscGOqUBUVwWj7%2FsXo1CkVdrp4gJ5fEWbJ1EOdP34bMjNXw%2FNnZ%2FH%2FkZROg55tX7Kx1FdBY1m9zp%2B%2FSHIX45XffjuOiBVpR21whtW5xYWsnLauPqytNWPUhg%2Fx%2F3quFh9GXlAmNgnDagXMSMIDXnKAS5Phd77rIuBMzgxAiYkbj2QdWoxIl2gOFSWRjfjF1O6NZXVB5ENKFFseddEVWYCIuRhNr9TSMD%2Bz%2F0&X-Amz-Signature=fed265df57936bdbb1f4bc3c90955e75ab9c9bc3dd10f860a4d8ecf45a9e1268&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


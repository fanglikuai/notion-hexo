---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVRRE4X%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIALNNbwvCotw7sUyu1va1Xk23rCFMlgHx2Vj3M1o5tDvAiAhfJyQdR8ErzVHOm%2F3GlWbrfbW0dV49%2BcTJNlvrx%2B9jCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMfl7%2FRED75odPinPcKtwDcJFwXIwj7cyDvHqf9GgYec0CsSgfVjDVdq2FeEnJ7vYPvZnZL8xXipEx2c7XDExEdL1FYYrxjKH7z7l12OaNLZRKFm7SM3zxr1%2FUhB%2Bt9AH4%2FNDWuAezJOaHBGxE4JgkCRwgsvrzTAEaCXTaB%2F0qpL8cepurXJqiY4o7zqDVqY6wb8WqI%2FkN8pKLB7J0zpgJQ22X5Lx%2Bd%2BNFhkIOS4nN6R7vhpKoYqI15oWZotaundnSMg2BrIHUDEKsbrGalDS7ePlPHfOlxsiYq%2ByQzWLbaPQQ%2FWe6T57O3hnsq1NZLLV1zr3q%2BG9ugtvtI2kmD4Nm3iJDVSYWhjPx%2Bs6O%2BTN8x%2F3sRXA6%2FxeIpHG%2BOp8AFGUEA4vI4RrgXygyXxib0SHx7YB%2FkSAYYwj6lilKPCv2nitISUsVHEaQViL4%2Flt%2BgnYXOXCkIUktAZAoMsu9u088jD4x45ciHmA9t%2FGug14jGpIXVCpEtPA54IGr5QWjLdIVeg5b6xkMmsLY%2BskxxtNKtG%2FmTi%2BCr6KujXX55y2ljgEU6cinvz6Rxl%2B1Su7JIiHaBq%2Bge06jKUdB7Jz1xjfgEtvSCxpK3QwNQ6ffx1r2nEW3BjqXy%2FRMFRIWb5WUctf%2BLyaCX3JngAQJBOgwroDMxgY6pgEhSArYwO1f8fsU5qgi46or5093vQ3OI2i5pt2bEQlwUdqeJyahSMSxx98AX0nE86dorge4NOem2m5l%2F03ne82Mkt93IYLQsmg6JabHtAcr7En8jNt9M4NLRwqUjCpeXszz35PnBr8b71uB0OqJO03WizHNBUPqjzLtlvBGC8M4FLLcIlNqKuMHe0TRqm5a0fT%2ByPWMkWLUjBW6z5Xh8yPoNv6AAgJe&X-Amz-Signature=ff7c99f54eb03e8875200ccac55fa1bf05bfa34fa77a5200d60cdd04c8caff72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


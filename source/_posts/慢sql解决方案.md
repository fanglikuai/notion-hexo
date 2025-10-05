---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U4X4TKSO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCoLEpTkBYGL3fezoB1RMw8xa74zjRltNJEo0BN1GFHOwIgTH5vrWy%2FZ1CiV1WzJvAJFJBoCpcuddY7PWlkMr2Qx6sq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDFEkEGn5a4XxMoshtircA6bDljqT8vADuBECZy3ertMJSgwyWn7JrtNIJw5defCCksNgZ%2Fzz264QploE%2BOCO4bmcwtEOeiDb1dgd%2FKscSB8dAuud%2B7XQpAdNB07QPsQQpJ0T1RAfTiQbmwjKL3EQ0sO0dtEjGbxCU%2B14QuOPp4zuwMy32v60Qd8B3TyHOPlPynEmaqQHq0oqqcV3ploYVRJ0VAmNI%2ByYkeBdM%2B%2B3Dyad7uBaK7D5XYQBAsGKejWBFthm7vo1K5sqkP0wuzrTTE0HxmKElLogtgxnmmRRPwXYKSPNuY%2BK30h0kozZYSh8w94WagMGoxFZ%2B%2BR60z9A%2B6fMjwXHDgifOISNycqp1o59X4l2HEvCGa6ayDDNaQiPpmg42gBbDK0Vohp5nkPe5tBxsuOSwRGNqx30HX%2BGyvUaJ%2FMdJesLdApWTSgFq7hZxBjd7IKEWsdJGk0ybdC3D2idNfAZxCgwmfTVzXVoHO0YCglazEFvqFZXEFwHXjcrn17hG%2FsSBiP9v8coKkr6EXoL4Ymz5rkGjriZEzt%2FmTxuxNhmt8sjlEPiMXpaOW0heocRMmNoTbiPwgTjuU8rp%2FZlhqi6I4QO1fA2CjxssEq5%2FNXYZFBg7Ro7UWo%2F0%2FrpcdPMlo8l9NHtG4luMP%2BaiccGOqUBRLJORkmsdXCnh5QctqoX2LZbmObmZFsEasbJjLjYmoaE%2Bbd7tBZJaJw4j0syzMhIejT7LOhphY%2BZK1MofsPoCHr6rv3NHaRSWZa47w9VdFkGCVXBb54YlqQ6kiczMMhgMFgDeys55%2BXSDygqosoVsEX0nNMWymdCzoVnp6tKxhtoN6%2FiARFFb03KdKDeoE0j4iUtt5ZRAx2WRzH%2FCf2I%2Bk04Wz3w&X-Amz-Signature=d3de6fde4130fefb305f20b4630a1bc517ef358ed8c3b96b098bd11fb7960f5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OKUQCTN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaTj5avkVnCiEpuMu6r%2FQJ2AgICsO1LMdYp%2BcJEUEHRAIgB7SND8kY58V0Rs4uCZejsqM6kevLMdA%2F07zyXOPELn0q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDA2Z%2BTKPabZeAKLeeircA3Fa0ugxzP%2BD6iQ31rfgkSaN9CBwewslCFVgy1TsXEazu8%2B2aCgytWE082N7JgdzlZTUuLaTRmAqkfe8J%2BWbhy1pd3VfLjS%2FAfOayy1I3MVGZ9W617f%2BfmyLS5thFLyhCZjwOzZ3MK2ELW%2FOZCtWMiartc06eWKXwT6boYNA%2FIXUTLug%2FyrLRt3fcdGL80Xpnk615wV4fSlmWNFZ%2F397kld7Sa3yF7UdrdnyrQwGonC7as7MsP%2FXwz68soJRZQwFEYDarsGVJRjHr1ubJNz%2BMKIzuKdfFXCbEaOjL1PRGptv9E%2BIUUW7wn3b65Gtf1zTkWlOuq9w8jo4io3vGK12czjLDx%2F%2BtJxjKeLIuGTtf6m4go8hMGRP5lDl7YYbSfyqb6P%2FjIoPKLcUbHAwFYfWW1TCXSpDR8pCtIpdxnD5%2BrIzpYVGjv5gDMUMwSxtSIvA%2Bf6q0iz2%2F%2Bj6RQv5D1cZcju%2B7HD%2Fgf59Hwo6wLH1QTyF32dPhn2cA%2FgO54P%2FCvUTogs9MN7EuiFc6LWGoYmF0ha5viurOlPsK8Z5M4XxR8OyDKdrQv7oyDdPyn4Reo78XjfOOdWJG%2B%2FR%2FTeICHxtvWs45NAtX7ERb7%2FtZHAy4Jh3WRPArYdG6T3o5Cu9MPPhxcYGOqUBzzk5by3d6ddbm%2BFTnUtZMm%2BBQhMvE%2F8fD%2FpE5xeJt494BXTBembjgVnBkmDsLE9YFXkt1AKbtcKLIsKySeyu%2BNUhGJZOgEZcJR0rfhIGOG35OhUEqrMVjdeolpIX4FUR0DPNmWLFOStfum0AEXOlJ1H5AK8V9Buzr6Xn1WOEpIcWa%2FAYQM%2BSzV2QOmbVWgDJRq3GsuQqDlBPMgkaCOkGF4AxtoIZ&X-Amz-Signature=afa9a35eeeda87d1c3b9db67bb7bd67d09fea436a45238746cd58aeff6fc497e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


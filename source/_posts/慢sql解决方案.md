---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVQB5FCV%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWi4M7Z4YZk8R2fuidzS%2FCYTrEfiyvRs%2B9M8W%2ByVqO%2FQIhAPAFK%2FgcxtELVnN56EFaN4tKf0WA8F4VIIsf0oQHycVCKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwjAPm5Vun2EsyG7Pwq3AMRT%2F4kjUZS4cBNunTJgfXilRqQa%2FLZH%2BveLTE4IVhnNdQACd7hmArfzH9M8l%2BuLz5SM%2FeJsOiHeMlULZZclLs2Hsz0Qld%2BLCQvOXUpjjGpJspdE9sbbgL44tGWsMdv%2FG%2BpHTAL2VPoduV%2F5hIlUAX6F1dlhmoTOJKd3iyVX7kfgAAJLyUWPtd3erU%2B0MjrQ5f4OBURvMgQWi2kNhC%2F4LFIbtBAKhizYhYPWAd7E0Fo5Jh%2BBrFCEwU8eLvTXeVFSP4z%2BRBq83gGcRgpTcXZehowZEPj%2FNbJuj8gmwMIZn2b8gmQ1BB%2FVCotLpxgS2EdFJWtADkGvRmirSAHkrnv3gMcK4ETptUW8YCRom6NhkiWbXkwwa2Sc%2F5abkLqRKndTWfuPElVYvTSWW2KP2bdp0eawRMe%2FLPfDg0NFBa2Za%2BW9MYYMjxtIjPIptvnymRadPik2aPQhlf%2BCgIHNEWx4xUEGd5RuI1%2Bcpl%2BWT%2FRqCcI9CGPP%2Fgii%2Bm35WMAqUdcGbtd2q0ez%2F3z9kvI22Xy9xhDdAC416oZheXKSgE5JvdV7nAmym7cxMykFtkqoFCJgFSBLNc8jqJgas%2Bd1cQKuRJtXZVbrKkmdLORdLEZozY2Sap9AeEqz2TeJx43NTD6%2F6%2FIBjqkAXgmf6E9B9DirtiG7VcHdNRR5RwebzMUzUU4U0CU6dMiOyNq%2BR7AJOsBSr9LjxyRNRSBs8U70cYCoWiZhSB2uj1YmQiiGwS3wWq9fmkK01ZeqiQUzEKwFa4Yh%2FQo4nvIbo9BVTMwIUiPqrEUZs9j0ePweWXtw8oQ7Bw9MF7%2BQbwWf%2FNnkhmo17agMYjUEjN083gEMBHyqSMouEtirUDTHQaNsvxB&X-Amz-Signature=020cb1e874b3b4de5a080adefb8b651e56793377d07a0b83321db72aff1dd486&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


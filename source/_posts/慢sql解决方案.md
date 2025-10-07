---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7MD2BYU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQC70BHVX5UcYt4xld4Bq6gkdsyo%2BQksM1IWliEXTRFDUAIhAI8ysHFy2Q35yuww2%2BO6kcEMJkfjaAt9Hd7vf5eUVx89KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxhnJksQuOkV8mPdJAq3AN%2F5IZ55Wxj%2BigJUN5Tm6tpbpFvyQLX%2FmYNr6ZeM7japDG6LL7c36DCnUc0%2F3612eupqRed1XnnJ6n0RG8WEvrysb%2FNuqHYxSHAAOWH1tvnwhGWqox%2Fb3SayztEmYQchKC5D3PIr%2BhlhMH7KZPMpWUqdPyR9g5KAmnYjyzomK3Jut31vHOUFhWwGQTb257KSIdJsWVuXFskE3ajldHL6fFI6Zhepb%2FdLeVEVGh1Li7D3NDTW4E97kDgUFIv7CBOsublADWpUgKYYUmT9SPAJuZkXWjLjlG1sW1I4FkMkpMpkI0niovAuUYcyhRJ6yhQDQbTVhIp6ByOQYUg9Dto2sdilXACz0FAwIzHugvkDpe5SjAEcDtg4ROHPLmHwuQo79vkMetZ%2Fv17nCCjUY2n1nE%2B1u%2BgDDlVAjkp%2FshHhwZxA5E2mScvDL9y3cAheYGrLW9D9c4uyUupkm32vrLj0tLQ%2BIvqDLvqn%2BMRpt3%2F%2FvCpCQfhuOvoVaKJGzLk4yYeJDB%2BKC%2BlhTl6CyJ2Vz09Ad2%2Fw5KZHxwnV%2FIVWjjwyYYZbtoPM7OrRy1Af6uKn4YQGgtI%2FMMB%2FsugqSIDoTqAyYsNVkAZ%2FpFLFfF9ODDr23eZUwDDaIHmRBrjikJfQTDXu5PHBjqkASaI%2FB0GYwFMY9FvfkJ32N%2BENxCyD5ZwZs6vUsctskwDzSlMyju%2BkFvNzxnJ243HERqUVDxqR%2F8%2BJm7Nxi5qjhe%2Bzrg5ML6mJVThnVbJd4g9%2FeARHvmOe7PqHPNoSTDnmSMGJqe10Au9JvKBR%2B7jaT3f2Z57e3ak2HYHH23R1aaMSTxINiU7SqUTzaLn6p41eRnLDZynLI4xLmquzF2RwbDajZVi&X-Amz-Signature=d0aa3e1e785647f33ee7d680d80e2905abdfe37da8d0bc918e98861fe036eec6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


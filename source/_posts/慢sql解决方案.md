---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UW3KPG24%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T020057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICMdzFICoZIYFCnD8TvTQ4Zn0L7OxafwLze0b8WT0keUAiEAublpNZXwCea3DNS8pQMARoGQHYUZ9IgP7hvWw%2F%2FrHaAq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDLrE2N1fG%2BODkvuSxSrcAyX1%2BLdDFa3SoWGU7iQ19XjgIGYyCdUh0D598vHrjMwNmfBGSybkNUBeEVwLd8UMR0kiDG26KK0Lq82oYraDaJ56qqJUu7J%2FA8En5qxcIRyNPkFd52qh9cHQ6c7n1DsZ4wDM3OojEqViLoCo6qV%2BFoCYd3Fej2HX4mnx96EfFGv%2BArGBcn9jXYnNcY4Wkr3DzGpQC1%2FFgu0Q8vDCJxk%2BynOPbIDS%2BeVr4pWANKrEI70KKZIhYmpePDhJu4XjbEkhLLj%2B5lJCd0MQoVQbH2Od%2FF%2F9NKjamlA%2FWbdTzhUCN7Ufuh04r5SqcvXOvjLTsX6m79CkgtJttt9TYT9u28qKOTTKQ%2FgY5%2BlSnBIvqfiRaix1xz%2FZUy%2FFJ2jjiEek%2BaySvlGGlz00nq77iJcxOhppuPki7Jd9M%2BFSlTL69aqr0VHRihEcJ68jvl3yMGzjaJXoJKlJsjGkM2lfgfNiOQkM0X1oUsPff3fKceV0huWRTGC9k7xiG7TePSd81O472PpdlxMNIeD5fFfH2pTAo3fZAZtxctl04v6C5IGQydw0wZykaipoZfQjcrJ%2FHqC8wFdjo6XYdXbwgV0RNVGaXeecLQIFkkHk%2FBwjGwv%2F78fpZDL2t%2FXCiMF0DpwGPiuSMKXKjskGOqUBrKz36RsVIKKIIr0LwBlSSIX0a8GEy2NvixDEJSF%2FdnfzUVVrIuFb00mugUIt4YcCTk9xjVN8RzlNrrhVKOgJaOdj%2BwpzLC0I552OJd8Hb92UwlTKz2LhnZgKYjk2rR4vYZ4T0xoMoW1J2YhIH%2B8GLKl9xfuDP2M5o%2B3%2B%2Fcr7kZHp2h1qCzNLgi1l0wZy%2Bcv8p64WJS9RSKLxLR5xGL3Nqo7HRrNO&X-Amz-Signature=1a80e599abb666cb4db94b4d308ffab59172a8cfea1f5aa98700789e614fedfd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTBBYACL%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnA%2BFx2QVU27oMUxqH69eSOYvV2qXLavRUaChMwhyvzAiEAz2WFyS9FJdAoUHbX%2F5CoEvOeyNCsWHMRo3j8sMMYwz4q%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDAPPhIMKPKu2olcYCircAwvzFxWiDV4JlHkQuEXjID80mGrBvFqFX4%2Bt1HX2JixHi5%2FhYehfRp%2B2oNvQ0ZSSE8PEcMg5hgfu9k2HsGEzqvpGYCTrud5WtjOMnHjseRxLTjXxsEx3EsP%2FBPw3MIR%2Frm3qgDJ9XitRv3Xdr3LJQ13aRKFSY1NsdKUEjOZztFMo98QpgAG2xZHo4OJ%2F60vmmxD8aMamwKyjsUZXYs6V0bThd1redrONObwbYJBofhXxzTMkqBKYOI%2BWr0uxiEJU1hB9cJvIcicpwayN1rvbSjkzXKMfvjcO80XfHfWXH1abyoJk7nKuHrMUPfjUb8IoovMlbeGmEAPsUjlqawqrCR%2FzCrC%2BjUwSRglciGm2%2BRC%2FhUuhOk94AlE1SH%2BXc5CfCcGSiK6Oo0rDTFk7QDqn9NuHCD%2BK8lCme0Ezg5VMANDUF9rJkAqx%2BTvuPI2uO2cYyTmeQk5WAvYdsTvCOV9D38pfsep3hswvAh4vzXr6B1sYxom22GRDbzlCQedMRve5o4omZ2oIwYov5vDkqrp%2FErrv46DQp4%2FPXbdZaiukViOtYcs%2Fd6B1nNm0uy3u5Ok%2F6KmkT8%2B2nMIjs3YwcGqnqEuXvT1pu1UH35LUAdmHK%2FbDdiqsPJVSbmeaA41PMLviy8YGOqUBWURPzzBKZiKdN4vILEzePSU6ZU9%2FjKe3Sro1po576NOSNqUU5IR4Boy7iiun7wu6J%2FP5upDszx5lUW4s5hTtx4TDokTst1mjUYFcEYKbo2kMsCx1cENd7WxBsVAMq8JWywpuXMa6TW%2BPgW8pZNS3tZpNjUz%2FkTfoaJf3zhMwzr1QfCt%2FnxaXrR05bLUc6PW0F49gKNx2DWvxmCIpq43fyNfsyePs&X-Amz-Signature=33cafc88a04c727b39ebf5d4f5ec7719459673566979fc59d47a5e1f3b611beb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


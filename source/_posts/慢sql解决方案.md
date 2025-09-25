---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTZWPA5%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDbVnYgEAVMy110K1qUFDpXcuAfaRgJASfT70C%2FncstEAIgA%2Bo4Vy3wdTykW9zCPSiq4tx%2F59sGeLhcGFLGGpRcUiEq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDOMfvGlHBLEoQCfKpSrcAz%2F8mQ7OMohyqvjo%2B5JCBx3JTU3bkBGmi21%2FcqsfT%2B5XSDMiCKr7ySqEGVKkm5hHYpd3T3FmHxotuMDsMC5%2BrdbEn01tfxrICApQ%2Fl1jJffZffI0%2B7kHjtpHxKhu%2BGVVfRzHRBSxowq4gbYLtFB2oqQg7F0pI%2FESiGjhICQ1yQSfSYtHyF%2FSxuHjh3DfnsAN70MV7FrvrHt8vbsDpQlQWSN1Jn9O0ONxDT3uRuWXZqxpfickY%2FLjYqgCF4hv9iyRLVGrMKdbhOKEbEeWHt0N5JIiofAQCCWQUDcJi5rwBZDfIIW47106yetPdYwi7svh0j3oeRgwwnfsqrGE97pu4JzoaR7yqG1MEHcox8olEG2rlGVzrvACHGOh8DNCO36a1HkwT1IyB9rOysF24L3YVg4nWFXy1%2B0NFc36pn48D6BGrQ%2FDh3evPON9Ix4%2B8qyL2kRvykFY%2BqPBZ8dThRxCKYFn3tpHXc2DdFfGFWc3kl8GdRdTeGjzz5wtyui8c4ddGAuemHU5zci4pL1CSIJrqqgjV033DtG8itKk%2B%2B4IDuSJmUS73G%2BB1Yp6QvHykCLskrV%2FjMPdK4XYEe3yQ41H5x7Rt1Wz9sYZNv34%2BobnMZk8G2AGprVW3XHWTgvBMMWF08YGOqUBEiuYfj2Vn06TS8czEUmw%2FdtWoXCBBzVzdxibm6pAlEfntUs3xvAEUS8WJlTfyXMyEGJw%2F9al3vS%2Biopd4%2BIoB4YYPkLXqx9lEWsxzPYAZOjtUQzF%2BZ63K1p594TUI4tZLIEpBcEcro2dA6XLGwG5Vq2trhnc821OmdFhz5l82%2Bv3OT6GJEXYetConFNeLoW3d4fu8vWUpRB1uvSkVSrYZ1jCqZBN&X-Amz-Signature=f3058cf457ccebffedd419b3ffe9094049a9b4730fef281a5e7286b177dedaf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDCTM4H6%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEG%2BEuw9h1RXtV%2FUw4hQ7LVNt7rEiZ26g%2FhsrZWkpgN1AiBjdh7xZIf%2BHRtQki7oyeV0GxTR4F2Hn%2FBWPz946sK1CyqIBAi3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXfRb%2FVrvfjqYZmOKKtwDcUMYBf3mmbP3dnp031zSYeNsEYYuVbru60sbudOgdqCyYpsXO45Pv9UnClKXx6wjpwKd8IYwkKu5jVYPDtKeIKTDLpfj2ws5%2F%2Bb260gJ%2F%2BowMDPDJ81o47RvywCicgEfNMPte1bsx8Z7r2jVYczK1%2FRYtXObZqjtIccoIyYHVbcX2Ir3yel3bxK7Y2cID%2BY8KDE6uyNRVEgssyhWDyxrdzs46SY8kQVJUn4y6J%2FxNMEOKPquqx%2BFm61hrlBvp3rs7dcrHK%2FpEbPHxkNI0sIE8mse4AUBitEXR3nNshxaYNBqebIkK%2FtdnzTf5TIbiQMYcOud%2B9tXne0C%2BrXstA567tDU0%2FdZpJ0vLpVjjoKuNHz45NCrFjVF31fisJNu%2B%2FyDr8Z6EfeYlAYPAMdD6IJfxBdUJrZUJ8zeTuCcfpm%2BcNtVtF%2BNz%2B%2BkVG0JKBKBZU7vSJz%2FMUHu8gnKHds1iDSdqfv%2BENlGkBPBhZKM05RQyLYQfhW6i2eEoeBXdUqsT7Zp67gH1iw3NOsDQqAGNir0PkStI8baxeKjNjgwmdQ3tvsGpjKqUnOQP6H2Oan6rbtAWaJnbbtLR4HTyBsK8guNxBQa0aGtdvni1tWypVEi4SuFWSQkCM7XRbH6pP4wrbzuyAY6pgEP0ToI%2BkLzYelyo8lIG57qSdoWnoziTAGlQH%2Fu4xkiruMF1M62u3BDDssjxfe0Z%2FnX4g4wjxRRbtIMZko1WlPBLR2fUDEBK9b3O8IOiXTNpCrNadvQVVoAODGG66luy7070vUtGVHMcOXjaA1J0F8%2Fq2xc82qeA6UxJVm9sy3w49oRObHVAWb2Mvq2lCx1preTtPGDcNH%2BApx2JD1QSUPqldFtqH07&X-Amz-Signature=019ef086929f336edbb2195246de17f2e3134a1a9d312beaf579ea5fc0a44d07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


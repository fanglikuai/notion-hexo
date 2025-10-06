---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VT4KNQT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGw1iFQiF6b%2BzhdHTOKEvSzBBLGXNVfscpW8Z7f3VlNAIgLdTiitj0QemXNoHakcRlCDWEirCTyHKDAkLJNKU4AVQqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD8oo9pAIvZ8W6zdeCrcA2mUP%2F1L7epmXAAg4t3mNMzJk%2BHIsrA%2FeV%2Bc15seULjGetObY58iuR7MUq6cnsKWiK4mbOqu%2FzYW7naDF%2Bh3nf%2BNsXuuudJI1OukprxE8QEHwF45M%2FH9DmvYBioT%2BRsgjpudRSzdnN0IRszSwQbDD3JGRJyOydc%2FD05v3A0LnZBUe3hztFktjhk1Le%2BhTqQ%2FkG%2BIU913BmkAP0S9%2FNAq%2BtoRoE3bj%2B5v%2FlrM%2FB1voFXaJZvQVGBr6P05zRHBcHOA4X1k%2FPlp%2B%2FbP6YiZ4uprK18ZEK7yEsmRL4%2F4A5BCOezhkynKPuaCP%2FGzryMMrzJfHW7h9vohZrPY441S%2BE1NAnKTIcjbAaO%2FQGxM97Ik14mOKbUWbDPNl2OuMixupepM6n%2Fcci2kPi8JKsKgtdo6IFYxnTNEf8cl1ZeQUvVuiXNPCsxRGUtIxVOzpsKwXm96MU83KUke7ZQea6%2BTmKB1RbGhD1UX851tijvQzyYly54ImLU6Q9grXLxmdNiTK%2BeTwSwftXj%2F5v%2BCmIxjolB953kTKP8ucmp1XTEfe4qC8Drr1XPHOLdaD0vleqfJ9JKrkv34GMNtbJyHLim9jYHL%2BX%2Fjqgl2F3RzRa1vpx4Nl5e3dlQCx422KXEk47WEMP%2BNjccGOqUBpfCwuDsssyxeUhJF8Yr2APhsQBtoRZcV2W0bk2YxwMiIQ6uOMVN8ODHXSjp0B71xqrWPqoS%2B20wk4pG%2BgWJzZOG6gWOR1PDS3hxAuNa1k3927IjQCLQJkiOqEXHFQUEzQsCzaq%2B2ikpAd1G0i9WOqwjoeAYNrU%2Fa4ukawGuBI0oJxrHENNMVhZtf6Td2EhxE5N9q8K1gGJmKimu6sUDM%2FTEnnJ8p&X-Amz-Signature=d4f000b8c76ef95fd9474991fb4c3fe5a5f0ab072173fbcfe0460915222bf663&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


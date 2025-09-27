---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TFG4LAJ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIB0J1JyBk1yT1h8DUc921bjBxnMsLcdhLPR4QOnywYVAAiEA5WjW3S2OWsVwP6PAXpZB53LJHDNjkwmg7gNHRdK6qWIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBG7xPE573zwhIk3OircAxDrjqpos%2Fz9rlFM%2FDGv4R38W4VXdVuCfXhLIt%2FGg8GbTLEl%2B2FP8lsJ3cvL2NqNRU0z%2FJOdagD%2BZ8c%2Bgo07DXrsUXWvflBstVQ0dG6Ljb1jCSXVsPkffCrvkkmWmTgEqoypIK%2FixOgp27VrWVRr5oGDC9xUFNJ3EMndRVOLC25VYQyCEv4lptricSjfekrpp7100tRF1I8Gi0QpbnO1emSkDSeKqiBq3yj%2B38e6ilQlSRSmo6jtPTOhJNksSTvwxx2Y5YcWPInq2mZXx1DtMdgX68lxaPZetU1yEswYTf%2F5PfGKiNYUEQ6%2F8%2Br0JXezvLMAPINciikTADDvpNVnjA32vDs%2F8MAq01%2BLDt%2F9h%2F9vcU99Pw2mNNRP8%2B%2FBMaZRJJwBUfNIErxr4ifJhDhKarh1422eoQbJYBcq6gl694%2FLKwBsPCxV3i7qQcn7IZAMrpaymjWVArXLcMkT53PXvZTA8keaDp58HzvMdNbFQ4KaykM93AFBUcuxqrK7tO0l2lC2JSjfXO%2BMFEXrJATshoL1xV%2FNbAOsWIfavqMXekRwUbPtYOrV4jvkAqe9DU83RQ5gxPfbQ0y8Ubojqn7Jvr%2Fk9uqYADaUAWmFwnqC%2BDJm7yN2TCGlfY7me60WMLHc3cYGOqUBWjLVvShKcfGtDWBMIGG7s7LhNkYWybb0nojnnDd8ziWbYENtPou1nJnijOWf%2FnG3%2B%2FyPVGfwZ%2Fv6oAq92JyF2qAfRCqJQzpW9AWkK8UtolDnyIb6uUUIJXbqebaKRT5WAbhnZHcnuUX9G6tCg8yLphgEL4w1N6LA0GPDlnO%2BDy%2FowuZm3TGFLp40eCMg3usOq5Yco8LG5QuP0PZqYpcf4JbrZcpl&X-Amz-Signature=f8366a16327694f0e7dc4cd9085c351833577669aed0812ebfaf3ace0333d498&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


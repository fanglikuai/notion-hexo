---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUHKCC7L%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvX%2BXPreYLbTcVMZbZ90XiTF32JMtV%2BAjggmGO6AE83AIgOM9l9jCDXFcPaBxaHkqUDrK%2Bj1LfIe3Q5VjPJQjRAtUqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO4IfN1PzeCMFPQM0CrcAzUOghLBdZiWwBnBpyp8368LAcsEjhSafXtsjYbIqWk0swly79U%2F2b2UPCMOvoQex1doEKfCoHiNt0JDjeVKx%2F%2FkzbMqC0Rw8IqfTgNZ8M9rXT0oCz0B%2BcR8MoOc2%2FWIcA4VhgNQFoghKutDu31qC34HbHdvtK%2Fvrr8PkgRPdNSjQhYgmVu7e3hSOFNA2WDUk9vNZfdxRaWiqgHmEHuuDMBpy9j6%2FaVv1htkxDkAnOKOgORnaVyDa7aRyUvWPamvMmy8kwExwqfCqbrCJRBeLUW1p4dO4JlW4tK%2Bv3s%2FGK0VnnVIKSPI78nW9QzdTp%2F%2FatUUEQhikN6rnGiG258KxM0WlfZHZ2abMjM2GbLvn4CUC4q7FknT3yWIrh8SG4eLdsvGiJ0TTG%2FchBHrUpMX%2BB28O1lLWsJatEmZ7F%2FuwxjbtjEeDnzgzZ2IMDFZItD8N27bi%2FzFxWyHTo9%2F0HMpf1Rme3hr2nNvsplqH6R%2BJ9uPT%2FEfSQ46MFUm5trJNd3TGkcNEf5cJ%2FoRmbJVMB2Q2psbxZjvIx7CGMYj2HdMO6kr50URLUmVjNeaF3v%2Bmvk%2BGpG7D3Rp8V7q%2Bz4ptJDLHqywpb7%2Fgu7UGWq5MWPcaqZ6PaWYWF5fsu%2FpO5lcMKe6sMgGOqUBOJsN19%2FNn1BXG%2BxCoinvWd%2F%2FjKPNzBbBXfNlnnC%2FpJGKc6NNRVW9aECnEXjmCl5U33VXwM56azk%2FXhB1%2BOYWLmNpdwbNVdoyjaZOZ62GYX9Cc8ARQ%2BHMS2tUi4YclB9tAH6JzlRbd3zomXGwB%2BTMWRQJG4RdlFMZluOzIW5RPxrlyL8%2BSBaeBhHZ9KJAqkUvjnMiyj7Tqn%2FyVjV%2FwBsSKdEVnLsz&X-Amz-Signature=794afd7f001893d22756e7e6992c01d7ea2813da8916aa95321671067ad0ed3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIBUMW7O%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdteF9vasAY%2BO4KDloBN2tYTk2SQsNTgqQaaIamz0LmwIgaVu1XXorcynFVgC7ubrhCFW0gYEw5rRdwsVmi0zQymoqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH9AYBnOU2y8GuONISrcA%2BIauJzEIOblYSZrms13ao0JdwVX3HM9%2FvcpG53JGO1BhNZ5zzT3NYSnk1SSZzMi5PorHMDy%2BD%2FWMbKIZMBiKx5OWu2Ki87PKt78pbABJWysODQ5ett2xF5WpLkNFifPyiCbgZZlJErcre9W7BwAonBIT78qnSYjp5CkOLm0KFhFkMc6g9DEA%2B%2BOg9Jaa%2FRsHKqVYlMiI3czgTHl2hmRtFauK9msKcwazoEnLy2DWeXo6c5oB2ZQc6YF4hKBlCeiLPte4jQAguDyQAHjC1H5UfdJCSyMCxVssefiUWIWpBc9B%2BaUg5URHnv4vFEzAHFs5DoLhSTuyMQ1IU8JSosE7Urn8c%2B%2F55GngpzG0NZpLAN0jzU%2FyBKvV6aVaDuPkB4u03GPPZi%2F71dJ%2FgD9T5Rh%2FBzHDPs782dFOFuXlBKWOH%2FGJyg18iCle3Px75fOd8WJaPeoUjp9vMExuuZrQLLLsiBmU%2BlgZ3R38nCI%2B%2FDda2TaCJPrXjHIfIfQss1ZSIzWomfaSsC1N4bYQAQOBSE7FnqtkcMffpAOPf3K%2FUCMAnm9f4NESFvP4WOHfHV14NybaPWpV13ZnjUxYgajQWzn3%2B3x%2Fr5%2BnpoHRQ6MPqyAHKW5iswNmkAdBxjA2bhxMKKa58gGOqUBK4exemkSaHWMfYyr1nPtQ24RgnnWWA5OfYe6EtQrbI%2B9aEoyQnRyM%2BLTxHz%2BImeos4zToJ2S0JKzuE7TtkMruhPddqr0TxWOUgdau5igTCFXN6KxkDeflPEyFmb8y9jfxB3xqT%2BNv8niCRqVjyxIVDsSrR3b8MF5%2B0QajFEtXJaDuuonLFBz95KgEG%2BtQEPjWGca1XsvqyBaHPuzX4a9jpuqWiN9&X-Amz-Signature=f239a20ca904d95e1845fa8739d697736e1fb1daf289473fcfcb9ffcdb2881e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


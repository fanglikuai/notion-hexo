---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7CKZCJU%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3HB3JJgpWQCAwShfAFJ885540iUovHJljdCmlkq%2FgBQIgHBozo7mqKayytT0J7Ab63UBnFIBOiC03For8qMv0WigqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKdQ70ncmN1ZUet24ircA9o4b%2BL09C2FnGpVOVuMm8x5QxQfJjMz5I%2Bk4uUKed8XIGR%2Fsj%2FTO6jg%2BKn2Rtqnr06%2FFhYLcqBEXD4tFYsuJTJMIVeiXDaBEFsJyWoLXSe2alSVCpMoiMlGT%2FC%2FBYc7%2FQ4R33OutA%2Fzl53CemPkVSVrzJnVK%2FYZYmBKsNG85C1d4Pdpg%2F89VFoi60IZw81MbBer8ROqXXELRhNR4IP93%2F1HlC08m3HEY88V28keajcvxKAIMqYWb8ckET5TT2ukoxHKHZoVpDbDPd7VFhNxS6TcykAmcqFknFPV6PE%2B7JoPHV%2FMGtqFGeXHqP6LpoI7qXSVj0i%2FUqlr7yVY8sLsHYeYCxlh9PjwU3o%2FeU3pU1%2FuESJYWOVE04wwRtdccjhnvTimnDAxO5a%2FsQKAutbsBLlA%2FKHKdRVQmKLoe9GMcWs9DJq3dnbAKqOM%2BxMfN6YNT4iX16YyEplerQLtVe68GcOvSaF8Ii%2FsSdQtBn4FHWOJP%2FQSidtaBe3wg9ykotTd5Bq01%2BbjITyD6ZmySjXAPSUD5s8RBN6gkKkjm8o%2FEeSDVYfuKLk8uwb%2BVn2NINDivofMRrsbJ08ma3srYcBOp679oc%2FpHw9O1hgyXkPChgjNzPME02S2IimCFEzQMIT0%2FscGOqUB65J4x50qkv8K5RnycMQ1Pqv6X8oak5T5c1hbyl7o13xrcQGdihc1O8dBnzNjYwwYWZ7yPQmgalFq7ucawsfvONBmDpurKn4XS%2FW9vZ3%2BwqshHpMVn0wVkmzSXULQJ5kUCFbhriACet88drqYfNriA5uSWX2txynDp7SDEC05OyUWaztYlm0cLomlM146vEnwhHKOipXDAMYfkYvvpHdNnswoJy3a&X-Amz-Signature=41bb631dabfa5891ab733318ea3c012870ff7c8a05f72134277c55ca7f54024d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


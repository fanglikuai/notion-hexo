---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHLHPLZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIANR9QDL4dBUGvlk4utpFxAI3hzAIVALJRZbc186S6N7AiEA3%2BwSBVniUqjKPd9N2bCtQL3WYpR25wX9rId47dunMLUq%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDChhenBz%2BIaHXeUzBSrcA4tASmYOCPjwm%2BiFvuMPqUC6rCF7HaqFp6f1lLbq4j1yCk7GkkX0VOBN6lL2GOG3NRkoB1%2FF6qj6ZkES0F464PeuvfAHLCaHGFIARCXk6QWVdVTUqSgwpPiNEenB%2B5TOqLgnfKv%2FJylhfoFaWFQQr9EeRmdlNWf8cH1XatMZDhCZrE8Ilm9%2FweMij%2FIzQ9%2Fo%2BjJsIA9K%2B5eUndKs4FN7C7E7Iy1xGP%2B5FOuB9BE3%2BiYeFPNQPW%2FYJTpU%2FLkeZF8eHXelwOQHD%2BTzVzCz9PvdhcXBCn0Ek6xkkBsLfM4Zvidf1emlNbqEjHoSwcNIt%2B33PiaqCa%2Fq%2Ba73N9SSdi%2Fw51idMYwDQGZpmgmV1XWoycbMGAlFhWEBXXogJQ%2BVO%2F3o52WxSMG1%2Fx1T%2FdbGKhLVj9hyIjA2hirFQDQf9NB2NJZnpfcraUOnTxrvj85bFchNV6%2FvsUesTK33PKqG2eAtZHtdh8p%2BLBPOr6rugHntTwYr94Ed890538QsTvmJ7jpIx0Bw9ADbGhxKlWrF2CtscZGYxSyXBr2r0GL5nhdgh1iFt%2FMkCoxiM%2F24HZTHVmMHoM0gCqPrVoEw9S9bfDp82MeG6ZqIslaaDK6s7WUz96kr5fRK2o4Q%2Bw3s80P5MPC93MgGOqUBM6HWhsCERQaYzgKSAeT7wznKsFlXQi4yhwpVYpgba7LeMGXN%2FBx7ZsAOO6jDagc0Zw%2FUI0PwImOC%2BIoi6c5JGHT9hDinIFuZCi%2BhroKofXJzbi1iyoAKPjYRqvMEYou60bbPdthF31iSPj6epnRsEkkHsB1lXfFEFG5bP5Ufrl2oYIy1VrCzT2UcbcPirH4fUfQY52xAYkTFBspcBGnzgnXyxZDf&X-Amz-Signature=aab7d75fa551df322d76c1fba1da2477b8285708d1357a2772a0c2e48201e388&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


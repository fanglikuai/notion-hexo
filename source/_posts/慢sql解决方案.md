---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XPX5PEK6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEnO5LB9U83joC2%2BRZEl8qigAASakmbCZvYvrnozAN49AiBdLViB4G4iXBsXhMBPSscE3rucF3XGl6jTZED885gRqir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMRbJlw3FytIx%2BN%2BssKtwD5cjwWrAPCsJoWhmtDo%2FNux5votYCCfA5nMyWflbZtci4HfwgUPazi%2BVuzp4d%2Bzsf5BS3e49BGh61YpvbHjoretmqhjjjP%2FUyBpIl%2Fuc%2B%2FhW275vq9eA9iy6KHSHKi%2Fswh%2FCsge5OUU74lLqtfWbwOSb7RgTmUFzXq9cjajEMYHm%2B6xBdQoypGbWsdOShqMO8%2F%2BXEbh82rgydJgD3T4cus2tnNUcaMFxfU6hJ9XSMMAFl8LQQJmFjZ2y5JeMZ2%2FFa3dUFk0HCyfyJZIf0YOr2wTVn%2BrNhLU%2FhircuaVuMWSOVkf5v%2BNdtMgLjM7No5pRSSh%2FNT1hEfBsD7PCMVMbYCuAFwReOkeeeQQrqRYlhMx3mRKnVAGWua4uNNoHHLdW%2BwL3%2FUclz8Duyr5dCxn2ZX7YjAMcLXnTLf6gYP%2Fvy%2BWj1ucCDzrwThivXh1ixwM%2FEL98QhSQlFXXbGPNTVtPn6ylG1DIzmTj5hEAnzjwRd%2B5ENrBaQF%2FWABOGZM4JLb3ZDKmHvUnBCikwxhSS9RdqkKu5dHDfyEf2Tn8ToP7gjEJpQ7PdrqTLN9ZVhRbv3SCDxF88dxi22UgvbRC24sTyevdrs2fWRbETGbuj2QyMBHVSWCsoaZDf6i4oPqkwt5CFxwY6pgFUceBT8WJQar6HCM3%2BBaYLFUVC3A0UhonSdWAk4HZmDseHsd1qVeCJ8hld6CCZh5eccSzfBrluWkBKctcQUZFFi8KWeNKxSpMILwa8nC%2BUYWMIB5loiyS1RhqKBKiJPXJwEqJpozUo0zkHTFDJMqTMKcjh%2FN%2Bni4IeU9PByz2SAN6yV5GWUOKB5A9P0wJJCMMokVNR98PMNlwElwug7rZcdEf%2B2V9U&X-Amz-Signature=b570855e030d9a9fd48abbc405a1f0c0ed132630946721b3053cef8879438de4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


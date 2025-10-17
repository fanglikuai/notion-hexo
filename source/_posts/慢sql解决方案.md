---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662HCSQQ2A%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJIMEYCIQDHrag5Ka4Hm2R%2Fuv4lLgXTHFFYCibuK3b9jT%2BgXQx19gIhALAHJJVz6mZvWyo%2Fr6JT8JwfqDctql7kjV2EHJQ9oeLoKogECK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww17CSBOb%2FkmAZxn0q3ANPhnfWCNPbXQQrHMTyVO54mzMtZxZu%2FXqhiySq9ZhiNR%2FGCdY5qNLAQ725SbUa8JO7INYPcdja1l7wRquM185yGuOZQDWNoPd3pTUF4a5%2B%2Bi3M8SzzVPbeXX%2BNueoW%2Bwb3twNbkdTa6wCjCmMvXmLPIz2yMhV6qm1Jd%2B8tiJPy1RijqSpaGJDBipHvaLPdeLmsVwOxM0LmDpVZCr1NY09sXHk3GckzEiwavXZNTL%2BilpGawRsD41HQw58pKOxu0H8HE%2BRoFkUTfP727xjcPeQN2EGyENkVCcLn3w2ETUeIJPrnma3%2B%2BYdou6glPrkLECzjQAzNizatOihzTWYScZAbgYr3IxhhuQ0nXglZu%2F6wCS55VBx2K9GUvNNFtUz2rcZwvRNwTSFVCySN7qQ97vNUOXJJOOadaa8Ok1xga%2BLksrURXie5dFH2CJYKoYV40iD7aeih316JIf%2FtCBUISAmyl7TFgcOVTw8oQ4d64PUuEpl71NZP7EogfQAQRLCEpzsaFySP8iUm0qRmOAaEOfPI6T6jCaYJsZE0G4dOsT%2BW44db7KMZFYObSDm8j9Jzi5%2B0jZ%2BfI4keRw%2F7YNqpN864Uw%2BCt0ZGJXdzr5wTYM28ye5WtvVMA5nl7Tf8aTCg%2BMrHBjqkAZ16JDKTmDUOstxI47cRoOm16AsztnoR8%2Fjn3EF09l%2F1PyowAYLhbsb2Zuz%2BSYjDNpo0QoCYsFDW4MiNscp9Sy2Lxpi%2F0WhDXIhf5AMhtUpBSbo0%2B9VTAv18F33mdaqMh%2B1cGu7Ljq5YDJEraaZQmhrhYiaRmw0%2FekDwJzkK0YYwuABeaXtczRbNAwTubGRvcgp7UH7byNF4E%2F50ezFQgKQAHuYL&X-Amz-Signature=e279cd77ed3f355147b18206428729071142c972112871930e2840aed3a37557&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


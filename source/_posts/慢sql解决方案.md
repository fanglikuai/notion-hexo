---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQLHMA4O%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T120120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIE79TOYD9tXCEykQVLD2qhc4QujYoq%2Bt%2B2wxEGZhubiEAiAlo5FCtm5slE0yzFeqX39JWDIPJY4CB2LhASmQeRV3qiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLIyAlJvnLn9fuZvVKtwDlCtB%2BqzbnjlaSnnsG49a47Sd4cpUqnlFj6gxUjRCyXJjNzfxKGDTlrxjkO7MWPtTxzYlUm9HQewu7WHPgsiovwZGMyk1rEymSOH2hKKTuxvtzESSJsvgAZWbu1AVB3nZvxwuC6YAWH5NNNh7yrkK2UjMBfp69NqS7agx9KjQgQyALWgRXL6pRJk8XNYwwz%2F7JCQavm%2BP3nU9yo8jumKsCA2kM10bm0bnQAM2LTHGo8kF9nk%2BgGWnAGmgaquFRgae9dHuQFYCjxtlQuQXRgdYAUttGwfX6Lnc18JxXuxcI39GaegdcCDIeLRg6eFJWA%2FB0etenlmlxm4aWPjOi%2FuNz6iNJjFotVT%2BqyfejleIeLggJHQL0T4skhgotRMo4a7a3iSR04MBPvRYUhw7eJglaKL8GKyQKOKQ%2BQ15iYnkAtS00AjO2q9EQWdNxtvNQi%2BVwt%2B1FA%2BJu2yDH6JfcljoNjGnNU6A3%2BIgdz%2FXRustiU1Oc9H025e1TjV%2B6RvyuapoHmx990T%2FGRTStRqGqC8qE2hnOMfWTzxWDhjF05TEcEyB%2FsZ9LIfOeLBs1YSty8bfOh4YiO%2BdFHX%2FEOYS4RAEXWNIJ8zemnbjA%2F81d3UxtOYPl%2BTlzFXwo39l1e0w5428yAY6pgFjse6Mvq6Bxg9wuGmhvQ8jjO6DIjs0f7vp4O1yy9Z%2F06iCPduY80%2F7C8oEOyRiJtz3AWIx5PwwhoNSX2H3r03%2F7c5bhvrXO7r0cCBeTZmXiFk5HHa%2BPwXCyN9j%2BMAi37OQBCh9MVCki1ChQycEEyyp%2Bpyq08qV4rXNkKvwHhcjPn%2BzEUIMli0%2BdjEzVarwmFs2sehoPwo2Syqy6Y4ud2f81s4nwtGs&X-Amz-Signature=8a7e4407d1b5ab6b89f4da270f75164bfa5b3653065a650d8fbb286f4b69c5dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


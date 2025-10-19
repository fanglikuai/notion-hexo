---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYCZQ3KY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHLEfsEqsgT8Tn6AiGICjOWoRraZ6JnlUfqIuO3ohVunAiA6o0qNgUlohB51H7p%2B4NMa7QZwdOjJgT%2B10TM0RoO24iqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgb7R8LFo2%2FpVJceiKtwDEsHPH5swfTDasqy88FRPmTeYFBvudKs1sEdzTBA4t%2BYbCbjxEIce22BDkMRBKx8oTbyZ%2FXl9sDYelBLx5CWdG4Rj8CX2QtosrsOTg7KL%2BhSvGyCbBeHIJ6DC1BeficgX0dBQA40olskK6PTV%2BHq2UtVLpWczMuk5VVb54fMeAfTvz2GCWG8b6J72N%2FH2sTMpWqokQRg%2FbBJoK9naNDp1hEd%2FKFYcDfbbXcCR2%2FS5gkWB%2B9pa3293s7HKnV%2FJhXpp1DPiMeQRD51jZuJjLKGboMlXwobRtAbJd0mF%2FA5%2BjKPLTiOwfgCig%2BMB6RDN4sT87%2FIS%2B0R8F%2FzCp6QD7LlnnHhkF9AqqGUGyqMViGwrCciKaK8%2FW0KtEAv7br2yqiGFxushc0c%2BY44ANzmc0kQDfpURpP5TUi7UGGU0UwOyYkQXk%2FVeioX6LrsJPYA996FPy5Wo%2BZibEVojPY3Tclo8zTkdWm%2BQKYTjgSgpKq7uESHu7fDKJBgi7FWmM54%2BNLePJf5larIQajPqqa9WjZg%2FReqAoKFKYJtuSwhnIeWHZvIv5xl5y4inRfwrzmR7Gdyx80prV6MKs%2FEJ2ErWFHGtS5OD5S0GTjm1viL02Bk4JllA77kfNipiDLeP458w4tbUxwY6pgEST9sQsNrc3VMnyUPrnzqBEs8YtcLCB7F3DJg5D7YEqSDFW2filhykcNXciJE%2BBRI4z8JtfQb2COOWmgC3v8f5hbPxV11nI0dr%2FSPkOiJhC29EVzAZ00TV%2FFfjtpUgrcJ7F9FT8idjTA96GHvClbLhgeSKG1h3CiT4yVT1yZOO6h0WfrgbOC8nsxpMZZemxbT9%2Fb4WJoOGJFXKlqdSdY3es40asLMU&X-Amz-Signature=3b9bd5896444e27c99ced828744c884ba46c6bf598788ec1e834943670389ca0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


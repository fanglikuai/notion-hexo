---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWL3KSRF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBuxQJFGKoyO8mIpILXY12zCHsdWcTwkIPcDIl5xoQ0AiAmF2yLOPu6uAF69cJlNG6IoLyjhwKkwDnAA9FiSEoSciqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKcwHhlRnlLAxTxYPKtwDZ1lUP%2BmUvRCr75FTLZDKLAehmRrFM6SByBrxWp00AVu79yWtuLO3Gu3Eli63G1nWAGUBWW0zBMzmAsjwgdEAjWH5Iy7LIhm11JXpKvMAxbNcC9U1Evdf1BXVPYHetLyF2RfJCLO1HOFAb4jIcoQrIsWicJhzQ58wfhVNOSZt59%2FhXrye9jVkp69gg7E0b9ecsAG%2FoElUdbkU5QGCNIXAzTDWUreexAvy1QvS2%2FbQx2bNlw647ob%2FnWGZEhuouITbIXIDfaZzRpomWlU9Wnl2S20WWYfEn32tVL30%2FIi4dk0kdMgzG6mxofCJlvSvadJvoMp0A7Kf0Hyz1wZqFajNJrRMjsp0A56q27y3fBN71uW6S2IllxPrZQf9zpfLgSfBZ454BG8FaF3zOvbivnZ%2FBZ5DD22Nbi4aSiV8cP7VLFlaVgKNwAsL6mJqTgx2c5hV%2BAh5ZEkHxyg4sTy4S04DLTtTJWS2D%2BQ1j4ld0aLg8N%2B%2FXKeLva6sTGCAzPSOlE9oku7JAoGdw0X2IRGCOuNDYe2xPYfhqm7NLJNGGB2VMNNOCMLutHjnfUvLR%2F23EM0Ec0EQPqFi%2BOxRoZRTE0qNlIf5l%2BydrTslcFKd%2BX3uXnHKURJFB1aMai4x0www%2FOGLxwY6pgFLWkB16aaY7C428bmKp1WEBJpZDUuV8wN2HEgpkKMmnf5erRb4lkKVtuIZrusT96riE66HovlRAzsl1wa%2BlZmlzqV9s%2BTG02%2FqWHUsYOILQych93HOaPD88Nvx919oJiGwohJcbBm7Wx5iMq%2B%2BjUfpM1cX1GkfwdLURXEY6wxFFnrcAJFSe4HXI50jLTaLERgP7etOIYqJbu23wrF99lG5rBfL%2Bzc%2B&X-Amz-Signature=4b50ac264a3d18745e22b17b577a82d0be041fab819825a1c93a248490b85dde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


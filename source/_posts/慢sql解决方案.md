---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLPWQ2X%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlkcWJ9cFD3smsTVwZMvIRFs7oJKJ55IbEYYf5ikP5jAiEA77nMYobYPDsan7u1aj8t%2FVDrFNLoctmVgA4HWjwfkZsq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDARZNjKMKSEYmUvEZCrcA4Rl5Z4bI0v2iTJqc2xjD%2FPCVO%2F%2Fctbv%2Bi7V3jk8%2FeHbcQ2KV6uSYsZwsxxU2nUV0KxEMxMq6zw4ftIsC7b%2BMmrYxzmgeoAGIscSSMdbJ55H2JhQyaIVkLzTCYbW%2F%2F3QXa90ooE6l3XqN1QEwSxECtoM7E%2BmGCKEH1is1Aq42Z1YGqNC%2FY3U%2BznrqEPT1b1k%2BuOFqErlSc7Efbj3Tf10f%2BHRIorM0FwD6XiAn2VhscXJfcYF7ZabykQ0ifJ0OpEmLu89gc0UiGm5%2BIH%2BDaCC0GuUuhZHS1zAwh1EgJyboPd2TK5F2P2gLPQmdtecP8ESPudnM%2FpFWyLGd%2BRNlR8cQ6RstST%2BZTk%2BpajPEQ4w7%2F%2B5Mn9FTt04dV6jKV2fBpFvSO28XmJJ9HjlwmqZqMCHKVcUrG09dBbiB7DS%2F%2FF0O%2BtWDMQDjIT9MnwV1DlZ0xO3XcA0gDNFZfF2RkBtgnW0m3eru00LUvChy3x%2FZMpLbi7qWs3yU%2FOzEGkVlIWZOHxnmENYKoG4h5xcDRXPBOULl8mI60uJvSNXNJi20R%2F8QNOcLUvYtACVj1uD5Y%2F9k3EQTRUzrKL3PJ2e8REHsFYVYxwOAyYadyGKN4Un%2FhXbv87NJvbP0lPa9tPvOWDKMNupv8cGOqUBfGwekJOB6CbiOPp0pzSDdEHl1VzRHeL4MWtW5xk4m%2B7PmXdchuXT15C%2FLpbp7viQtzDJmHvdQVY1per%2FuVmYJL%2F5oPpJlxx%2B%2BEV8UE2x59ngwkIrsenjgpevhT2hkTqyDNlWrWStSFy4A1vpiCIUkRMmuHbW5UaNE63sOqViziwpfiOviU8xHNbVPvLGG3dF0WB2w90LhtS3%2BIaWDFxU4AVMKOSy&X-Amz-Signature=8e6e472a3639a10ef9d3bae5a5cb645666e83d42b85ed286b234f90f77dfec75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


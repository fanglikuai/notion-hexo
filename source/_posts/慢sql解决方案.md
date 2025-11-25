---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SZLWJDQ%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBZZas9VgP%2Bkdcn2kg7vpUYNGPxZMuU8oOcXP%2FbQESI7AiEA%2BaE3RSN6QcHWmapQh1AKJ3TM69Vth7WhSR4DYnRLD3oq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDLQalkTrocyCpon%2B2CrcA9WeT9ZBt4gJS%2FPVby3hTXdxGUgCOYedhdmprdeIIHCzvBb2UBLWcHYvhEzkg8m%2F%2Bjci569sCFYXm%2BUK9GPU4fL3NaJq%2Fz2fb33Ed0l%2BW%2BHDryAIKv8pEQAdLcMWzaGRnLvra0VsEf%2BF73t%2FA%2FHH3Zr5Ke8e2n6elzgBQZYAj4Yzvum%2B5tcu%2Fu8HaxUd5uH6d6Z1Eztt73J3SwTGyVLbkHA5LYoV3thslOZBqVDBV%2BwaI2xhgGZDahUFFbnXtsbBrqAI88GbusZqsNNwo87Xtz5ZdhmKWfsHLCZYRV3e1Sv2UCkdxxHidunlJzQ5G1TWjMD2QCxSjUE%2FAZTNKZRttmXM3rBciHroYr6wRjxFOTjFDbWl7qEIM1VGDAw80Pht6PLx0wZE2%2FynwhqrIFjr7g7iOptVVMwdnUADoy3sosDNE3kGcGF3t8K2ht7cYISIoReslfeD9xS6%2B17b2uV1oCwRddkn200tgyDrhP303szComad9yZakzPbaSla1rxzMXzj3YwiMhP4iGOZmiKMNjMrxsHPtdGWqeIMwlWdkZ1f9zgbGCyIwrx9wLYM7D%2FlSx6MsEMCZOfe3uucpmWN9UgN1ufAM%2FCk7Qs8CIAC9TXKhdoWKETpONp7Ab7mMMmElskGOqUBJewfJ3GwfvBBeIdyiGnZdlFCges14x7muDcvl3uu3l3a2WLoSB%2Ft%2FjCPAkRVrT8hvIIhZ0x1%2Bi6IA15KL%2BrDkDZfuNoPJBpaA7XcSc3%2B%2FlITmy05ujasa3hAP3%2F8d8CTh8fFDcjQTeXKGl5VGhz5KsQY40WyCKgH1kWiM2SF2V4%2BE2Yr7cGfN6ENgJdzA3YPNqg3M5rXSSWzJDAghHO1ay7sWkGq&X-Amz-Signature=53b65cc1c7adaf879321a7f9309098b9e0bd6166f5fca0373758ea0ec8553407&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


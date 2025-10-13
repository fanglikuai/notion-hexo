---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PS6HS4N%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDi6%2F0r7VjX5%2FXGFCO7JrSaEBtV05Nrg7rxfj0bxRMUTAIgUkRiZztXuOJsbF8olXiZxZk2odjzg7aLx4tpR9j%2BoeEq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDBxQ2TfBfx6Ay4Vx6ircAyKZqUiOvr42Vm7EfJLSpOh1cxe8oHKkInefoOcX3B6mNGM1GuvMuAySkhTucpaeQogJfwlSLvesfJ7SM%2BG9cwae5huS6pWyZb29%2BSkHr%2FYAaDlhJW0KYFjgB%2Br02RnCc9Dl%2Fma2KAgQ5b9Et7cN2vy9cEqRjt6XStlHE7pmMgOhjkVIlHt44isP7HNoCLUkRnmGWzuHmlQ4c%2B17JyrrK1Gga6vmyYixxR0SdV4Xa2RY74CFOMioU7Xe4okQ6ll%2F%2B%2Ffg%2FS87NGiOE%2BAmNOiagzwybinxt9OS10FwDmXvGL7QbUyjQC66ufeCDXrWVu%2FYzH83h8eKjjmTCUxbg%2BTF3s8CnHYAl3WXFKfq8LI1duaPczjvygoGReXXtnNk86vuqqRE03mrY4bdlvyOF%2B6j5AHHvP3A1IQrEG4sZXbw2K7guGoppENSP4%2B5j0Izl0F%2FbkJYsS62ki0DWWJOgzyafzuJWrhqL4crO6ONfxv73QwB2z%2FgFKr6F2lTgRS91ff75NLH48pLCYSAgOiRAs5br6kAe1BTV4XCbKt4d2G7ca%2FPtmB%2FFIL%2BljZvv5zXx4TLolKCJ%2FeykMF7mCJ0CK8GrGDxtXrZEFe%2B8PgxSI3WjKvLsdyokavHLNT2kwjSMPzOs8cGOqUBCzYqQ%2BrdSljzamLvDuAN7YB1qpFETgKZWx09NQDenSihB7Cup4P8y5pWImozoWV22D5LNmzL1lkNt5f8QbNqdHN0Pt79863ZI61OLCQnb8xvljLnalvbb8D7NrEMIn1P5aoOEPstR6iAbflt5lcgyOzB%2BY7ES4401jSUAJ40gW2B0X2P%2FAZuYiOzb9bTj3K2O87QVrBe80KTTzZrOZq2DtRdW7KZ&X-Amz-Signature=3d1b0abf55a3a79be8bc161b31fd1a3a55ad5ecc3c119b2c932e24afe0b5ebcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


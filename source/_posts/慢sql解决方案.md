---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627UDHBGU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE%2FIIY9gDHoxGUNTgROBvu4BdKZEeXcYq%2B%2FyPVLPW9VAiATCQsD9PjBTk%2F2VbO1O687byfJLnsA%2FcaSjYY9L6BShyr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMYo6uPfeYjrQHMMs7KtwDE%2BIN4516J3tSdr7q3nqKmhyykNiLplufmTapmi7sTgr6l8MWE0iJ4%2Bt1nofaaGTI8m9YojnwUX%2BwkE2ut2NrwEEYsvLjWSY0woqhXdWe%2FWXFFTVIMq1Xlg9PbuXN16AUkVGNB9%2Bclvz%2F7J9defYvp4pF%2FA4AUqELiNefckQcotUv27cSESbpZKddEFQZdqq5kL2kYr%2BC6PIOLpgl6Gl1YYLRd54FMh0lkxk%2BHlJ42POFZ6o8tm7%2FuKvEabuhi%2F2DT%2BkIuXdg%2FdLpKxpQ4ZwxMLVUb4DLqol4XohQwkM56QC3ShTtKP1JfuwGAU6kI9BznIB6QJrZT5dF6Y%2BP9mqBWMc67Zl%2FOpjq%2FMTqjduMHdEmZmWQVUsjY63VeT2DwmyscGQjgwm%2FXcldl206ICLMmTmQ07uHGpQCZdXYqMA7yqvdFG4Hzs2X%2FCPwJ68ddiFA3PoDB0z1B0w7Ectljirz7FQMo%2BzlqW7VcTD0OdjguT0ijwgQ1P%2FDhFLXbgeBqMcA%2BD1uEQgrf8OBetYRRummVMczGpdmGshJBzPQMTBQcHy1V0dBcrM%2FqkppbjY1DDe3jOfjrq%2Bhgio1HTY0p4PTPJrHrvytf37yMnA1LshVqJZaB%2F0p1XVMEYbsz74w%2BLeuxwY6pgHRnDIOMbrClIhcefHjX9jlbg2B5kvBNWwfPGpQnA11JOVwN7n3PAZrEGD%2FP31a9q8BnnCdjZFG1R1v5yIqk3kD5uZmXrYwn%2B5tDlVzd0egRepYCJW%2F70ADabUZms8%2BLAERGxfNS3IlDW8B3qtLd1mHQsRPX7AOEGqrt4AVKyPfm%2BFmzxELS%2FeiG5hg%2Brqf7u5gRDNugfWZf5yf3ajBUAUP8Snl9Bpk&X-Amz-Signature=ad560e10e3dde0841e22f4363d33c4b7e7b52a537dd50dec63b462098ddd3f41&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


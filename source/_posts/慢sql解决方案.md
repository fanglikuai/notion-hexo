---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633O5X765%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDmu3lP7QrLWYopeC8cI2JHUPUQ%2Fx%2Ffk8mOfjHL7KUjbQIgZ00JiPgnSxVqoOZ0MZJ7h6vgBWW35lP23kLwsDh2ZQ4qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAiIyf3o00xve2GrlSrcA3vONDuXvKWNmykNqK%2BeEjw5FffHyqM3357aHSRN2sGTRGCEAnRV%2BrZEu6JJcBTe%2Bf34axLeQ6ZkC84SmGX8YLchakgtxeU29Ii02ic2F6n0TGogiY0k2HFDFnw5Z02X%2F2x6ioXCfMAaPBSlR28SJCQPb9aHb5FG%2ByczByLd05xk7eMzTKghR%2FqFVuTqyGVmReKxZ2peyUcb5A40bL70wXkNL2%2BFPjxekk%2Bk7AFCJcKSZwlwL24wLzKY%2FQyc9nvxGr4KSsfkWmKJatNr%2Bct8m8XtpvpVudYy4LWeL9Pt9Qu8tiJgokEX3BPF3ylGk%2FQQTupomsdRvtjsozJTWSchzwlS9b2%2BWAGhXwUZK%2FQ6gzu9%2F4vvYNaYF6a8Iiu1yCXxIpmJrCuVlQDIhBpyTA%2FdB6ilaT1o5dAt0OfHczIH6QQvnDffDtyWSSK9BWxGYYY%2BJO1Z9vXw0wUq036g5%2FJS2K6HAsaMmEtRKhh7GFQk97YB7z4HgEtXML3vTffglI6vBt5fhvxuZAxSE6cjuvK%2Fu5m3nlZhDmDOQMrHXGHYHmR%2BywdPV%2BArAJIHhR4%2BXWWVuQB8wIEVXVf0z5C%2B2LM5kHRK56mJXgHxbP5jpDGyPynzUE3lihmrAiKj2k4pMPOEyccGOqUBu0toGS2rkH8BkpDOYvttJIpx%2FrtPGN%2FYoPy15WvAtnMF19AB%2BjecspUbtjQqgkn6e54xZ3%2BkRPY4KtXy5PXJfjEjgThwbKH8zcz8m7dXS4oC%2FFbsbCu2fJCgn6zzwsLVmt37nRT125y9dUFBm4leAuHp1EB%2FJftf3sT41tA8mffBqjETN7c%2FIYfmZfB7%2FYZig0iiV%2FvcbSD%2BEdcUeQCpW2c907z1&X-Amz-Signature=fc77387ac02423046db24da6bc666cf0af47ee6f3deee438dc3580ec3d8bd3da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


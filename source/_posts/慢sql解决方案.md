---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZPUX4KG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T220054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNGtIIWIsKgqHH453edd3CxTBoJEAYvAgVoCryJfy%2FsAiBRO5G5j9u%2BPLAaV9I3zWJ6Dzc2%2FrZAKi07d6c9LG01QyqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMR%2FEKSPgE3v6zkqmOKtwD4Fy2pK%2BUC%2Fr8R1ErY1iIu%2F1P9fKe4TWs7UWeuuBEukUbmBJClDvvkHKsQHKmaG70Xr3USPrI7nEqUO6XjMO6GLG2g2qDd7%2FynKSlFQOcVwmFgme8Hq3wDo%2FYCAXfuaabn%2FHKuSw6Z5dJfeMyAoi6kzJtXO0OoGuc7tih2n5I8iOP1REbe6%2FsNPORLfUsj5wi%2BIMKIGI9HMOj%2BSuoFWwJlgxA8t9RsxrDKVozmrKJvQ2lE8mQEUC94Nf0S5MjHy6m3Tp0EU6mrB2b3wfn1sgrpCCr0InHtyL64Kxeqmb0EW%2F7GvLXyucKZjDggQOXcmpv9rJAWFIKaAvfWO1r8S7zNfejAYf%2FzDEQywHNNVmN5Q4ujR87XqKRR0sW9gXxCmOnP4pZcXYzGUlfcMrVJSyKtbP3pi%2BBGfX2dctsFy1Xix9LCh5qMPV%2Bo9atObTZbe%2Fxqw6gx1g5fuYb2KHxzZH112SFviUQjKlETIuOLqZtQY7%2FTo%2B%2Bw1uVOcaxoTyirEPD2fIhM4OLFN9rVJnghunSQqToDMgLRaPWGwkXHbjtHzA2NwJ0AD1wvjitjUqf8mDuRDfg1XDKBnMNhhc%2BhEGODhi9pRpedniGXAuZo9tedg49LHgc626bUdffPEYwg5D6xwY6pgGX%2F%2BsLmGy19MpLnfcCB9ZuEGeVgKzgrjC5yojZpqxJtJ%2F4MqhzR%2F2uzvsY9v3sEPWcg7MJoAZdL8pxLgh3z0pcRmRVyTeuCoTiISli%2FzvkIf3%2Fw3nnH69RxFdTBEpfAgE854rcv0WKDoFYkhCjmGXBXDWBXhVZmHY%2BeEr%2FnXI5xqh6ykT4bGpTsqW21xfZ5abdhO%2FJm1TC02zYUeqVbBS4EQmVdyMP&X-Amz-Signature=1f1038388cca38b972dcceaed2d8ddaea1d67be5c0449bffd921dfdd54f6a996&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


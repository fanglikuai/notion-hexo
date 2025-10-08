---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JGUBSOF%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQCwBQUSHlKKgoyV0ZQ%2BiNa7%2BSmIg5I%2BmAhJ%2Fon4%2B3ywwwIgHO1OAZwhttrX3X2TAKIlw5rdS7QhMLv2QMlK%2BGwagXkqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIzTuZz1w5Rb8Zw4KCrcA8GM4VOy69psHKNn%2FwDxJxi7CcUxmPu2rVJHtHmEoi6ORRp%2F33ngBgHJb%2F%2BTp391fCx3YtdBufvUyqZoerED2pNCthnBLH8pnOwTe3seQg%2F0F4gJpHI%2FdjD7uY%2BrQqtWhnBNjXb5FnUMRQ7oaTVx85kUrdHVSUQzmRiWGX6uMHDw7gH01lZYoQ18%2BpHlEwl88APFsXPbjq9vg27P5lUeMeAnOPBb4qeZyi18tS4R5Qg0TWN6tuDI4wmNOUYPT2t%2F%2Fq8Pgj4PONxcWqygqzeJG5XEbxSiZfoIva%2BUiyTHsOmScVe2pSCECFr7H9YljF8uzc2mSd1paY6EvySYEaE1%2FWUFe4IkPdcgCyXAK%2FL4BFAjwZTJh3BD1u5WBaX9bKnx1n2J60AbNfU604W%2FONNyfpRIapOW%2BbzxKuVRJtAMIkbncfFODyZVdhsqf%2BDsesoohjlZTlTjVghlQu9I3osQaj4h5nePNQurGITSqv%2FGbhZBcmF1EYL4SNb%2FoI0l20L4K96TElDJPS8eSEhYSY0YoTASx4o9v%2FdowZFdsY4uHOYy7rLbPmWw13pR1cFOktLK2d5i8XdcURwJqUr7mZOupUecAEGAvoFQT3gu5JTlPdVbs75V06gMeCDpeu0NMMDwlscGOqUBglx%2FBTRh4V7N%2F202U6ge0trV29igy%2Bvd4x8x8OJ0t%2F3vaJQRXrBPArSXzqk%2B5N4lIX0Ll9O1AbYITV999jrEoqaU4qgyuBvQGwKV%2BrH1wN7w5Kv8PQCHNQ2Z2PmMtg1QDQq8UQkbHFWUAlBKrHcSrtGhlvEKrkCe7KNpWo3t37PPblZyWEo9NCT%2F1aGuJ9joW8F2uvgv0JEkVJgCZG6xp%2BKsy3uB&X-Amz-Signature=8479be0a83bb60ae2c0c57b164df28a3cd21230bb4f336c59b621d8bd6e27e0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


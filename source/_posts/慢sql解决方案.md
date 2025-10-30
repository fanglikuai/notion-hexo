---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPDSA7JO%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJIMEYCIQD643%2B5fQQ%2FZoJpPXRUI9%2BQ69RFyD4iXCDecFBnwOgDOQIhALS5s%2B6GF7dx3KM%2BMNO2mT%2BuoRSCGwjOptLe%2F%2BJilEocKogECOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2B6lq76TANv%2Fkz0Moq3AP4e5L3woeMIlO7wVozVB4HMrgFbZyje3XHkpQqRrpXtMlK%2FbM8JiP1ACUuOFe%2BhqOu%2BhS78xSHA8vtj%2Fb3zEykNhWf4e8UN2opFD1z3jBLz6uXIDUZtnubWvjXQjKsZ%2Fqxm9vWFBydAcS7b5iO44OqSfg6oTmWQrBkp2i7yngLBQDEL7njfl0s96%2FLhkjb18rxgVa%2Bg42Yz17emrlx3Q9yKXU4wxrGMr8P7Qw2U%2F4Xj1vsCIlsjMaCKzhyhJPkvblMspyBWwCNq6hih%2B1VUbvsyICVjAyN1bTQPFkOgbFdS0ALnr%2FrbpRCErTIGNHd0BPPN9cNT%2BqaCLyUXp7Merc74GUFJitSi0EM%2BW9WC6mf%2Bq1%2BWfP4%2BERxlsB%2FqwnxqPfuVQeteHxjKi6dNvwg17lz9WfAKnXX5lCsfIRX4WEReXSpH%2BqIcSnNSNkDrINrOSwBKwSZY0dU3qC29lGJmgkaTq%2FMYjRiFTTUzYlI1VolBdow0i1IS%2B%2F%2FxTxbxlJ545Ikgf59QhiGgiuXaqyjTT%2FwsUNYehCLmlpXbnPC2ZmYPyS7QJsWUCut0gaYjpZpiByBlvkjQx6x0iIZdx83lVU1q60FXTe1dwB%2Fk3y5GAepirFLG9dETD3HPoF9kDDslIzIBjqkAVSjSXEH%2Bua3g7YvAbr2POoeJ6OOYk2SHhQqpkGoXrZBj7lMTsWTj%2FX3uOdl93U89G4VPxkEnUF04lMWeePFuSzmTeRG6r%2BKXS1lThT%2F2xhs1C%2BwTQQ7IGz5UzOu9HnLQwurKO3RdBP3jKXQhDuQ%2F9Xdg2EEEB7V3wONnnsQtGMjowGiiF7sXO0cAwIlQ857a290CnUL%2FJVMVrWCo9StfmgL%2BYOp&X-Amz-Signature=e9dcd1afb6b44edcc5a8d99d39518b2d4824d41af9d284da5db358730cab7b8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


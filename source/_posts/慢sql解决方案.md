---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674ZZ6J2W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICjfWpfZ45bNaqu%2FMVQwT%2Bb3GH%2BsRzVsaEW94qRVtI5mAiAs3Gc7L5EwxEbR0%2F9N%2BWsf5GKPujHwyVJegIhiqcUo6SqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOsIG0A%2F%2FxO0l8L5zKtwDrZ0AwYLQViaNlIPs2%2Fs9KOwzn8y1f2Xm9cZ5JWuB7KKR1XowcE8iJVS0uBsDiYzzq%2Btx3bPq2dMKbXW%2FR3Ch4HtrGlhij7SKOp%2F4BzcYrmdgAYAxSummWmDT77tkGtx%2BpMnsMy6H9H3ATkjmtah3h%2FF61RhvGr%2FBQEX6YMDiKsmG9cZPz0o7NDZLYG7W2wq%2F%2B9Rgd5nxroDlofPVCxn7KWTi%2B3zNjoLJykEm2ElobIyCRtKo0Ezt2gpTPXPR8L1C8R50%2BOKpK0hX0pSuE4ZOORzjugNniBgIFpKlikZaKVRRhCxRe65CG4buwz7w%2BoEpIdS9Z%2FO0IXPbZ1HHInphdrZeNta%2F0MIA7ZY45rC11fn5%2B3YvqphY04Pg%2Fkygk5sCtpeqtat8KOb1uKWPJ2Oy5uDtNF4zlU3d%2B0V2jVHTGVBWJWvrVFLhMXGhhynxumTdS0%2BcsXLmB6ejcN%2B6WHegCA8Sl2%2BcCnyv0CI1FEHn8by2DXFkS8MSyl2PrULXUFDe%2B%2B4%2F6greg%2FzTjlY4tEs0g8Wzp1848KnmT91EdZ9XdjXMZ1%2Fo1Fypxr1U6eupX5vvJVzhRR4zudVr02BeSB4daeE9H5vD8hjPxAlVb1bbj7G4h9PyISZraB%2BiHUowhYD3xwY6pgHzLeuUvb1eate741xfd0KNMd7vvvvdDU%2FsGd%2BVjTijurh0jesG6Ba9rqbHT80F18PB0yg8a1RMGm4vgtzk0xte0ezMq9rT1LxplMf2pZ3wsMePb7Ga8%2B6QIh1TNa51nMz9oL3xWt8pUoXlR0kGuFovjUiaX3Sxf%2BGw7vvTNnEjsMKHqgRqC%2Fm9ZHuy3hShi%2B8pK8DlAUvNqQnZNQsILb9spFL%2F9ozY&X-Amz-Signature=0e8deb726c5b62151ac127d63039f15d775d536d6d9c167a26b134fe77fcd454&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


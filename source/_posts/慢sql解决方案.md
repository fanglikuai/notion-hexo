---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HFJLQGL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDXwRfNHMXEwvnG9%2BuaPheaqL7%2FIOwUcY15V8A3T0KWxAIhAJxTslicFmizZF1IK4ZtAVghhT2Ao3e9SG%2B9UNgyEdmdKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzy77JCxrLDSgBxcmgq3ANM8T%2FLwskN7%2BHQVGpSF9fQXlGDRB1bxJqjt3FXrJq7Bfjx6Ts9juE1Z9i0xwCU%2BaphPGf%2BIqXOkkJpod%2Bqh%2BiwwX6SxTpvo5hl0De5F8dTr%2BD%2FmG%2F2N4vtHWx%2FxQObydaeDGEOE2PdAfalJ%2B7%2F4enVr0wciq%2FMpYcQ8iRWVfCHmgKCrp32atfkRrKyPnn0eefdSZqXXi8mrqnhqlyW%2BOZnUNYi5f0eB7sQNcAWEHBT8UuXtnLovGI64sy2U8JEwV5%2FgefcAwurs%2FWQl3N6zZbTQrJHQCd0sTIYV8RhtmjRRXT9L0EhNuXwfZE4v9%2F%2FWQVCxjyobvUMPFvV9uOvExXnxD3xZE9c1d6MrvkKMwFPiLv%2F0CMmpC0wo7pMMtVoqGjcuFmriYWBPpEoGO%2F0eswhfXAGPhfbDqYhoAeKUmFtAa7dAQOPnfR0Udw2xNuU%2Fz2AmfwUn6uoy681H6nCc41XCOrr%2Bj1NwMn8HoZ3hkyc3fU0l4sK50Coj4dxpbe8MHO391xC2gP5FJVcpKORkdoG2a2GBudANfeKJW%2FSkVSzzEGFJmMssGn1mcdIQfgOLVyNJ4gpsQgotRLLzCFXAxB%2FNl0jhQHnBFsk7PRyFemr7qJkQmOUbtOzTk9zlTDWj47HBjqkASwhdIh4rw7IkQZ%2FFHPiZ1Z3MwZfuN7Zbnse8Icurx9vcMuZnQ5JOe4CJsfyDoe2mIVjiYjYfDLYmWwPcUtTokeNdrLxo%2Bq5c8yygBMrH3KooUxIkuESnIABALui7bJ3s44f1dCKyy8w2QP8ynbTTJBf1IJRswcnwnJrEVgZmDbz6A%2F4X2LagapOH7s0elrAXkuUfMfrUng27aWLAp8mGdSESXeK&X-Amz-Signature=bc40534eadcd6eaba6752cd3ad946825438cd9ac99f1e7513234c697cd6814e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


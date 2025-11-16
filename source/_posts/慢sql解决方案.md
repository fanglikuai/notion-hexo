---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JY73VBY%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtxX64Jp3bSSCRinITTKrj47L0zAGI%2Bh%2FmH8YR31AQ5gIgaob7m55XQklpTXIQ7mnQ2xuXNwUom1ygMbOVMfOhlgAqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDEyTsqhmJmSlLNvVyrcA0EZOrYqxCLZ2kqzJsHbF4qYhtG0MJo%2Fnc25t%2BpEm%2B8qdj7jSXrW0cQhuOcvEpWyrwSH9CXDlcBEOuu4Ld2AVkmNFab1kXAi3Wfzpk5JYL41bLVa8s%2BV7%2BSp2vppCj6ThY%2BOFrLMhZogANzNGA1De6di1OwmHzLCS%2FEX4TC03ensXozFcWQwjlAAhnG7ojpRcTFSoYwi%2BGrNHRULLYzcaH%2B%2B5Y5RECnjD0M0W2Csrc9d1JIIQpNlLnyh2afAxgHvg0SF3PE1fYIJF5nldnm52NWNA%2FyC0%2BlRQG3CFLT65IzkEp8w3mgbwzZWrKLS1ti5iL7m7iO8E%2FnTRkKM9ic0phYMzxBZ094ZO%2BxmhQe8EajeoQDzOUcJJ%2B211sPL2Sz1IepcgBYHu2BDkI8sOZH0KduD8TFOyuKixAHzDamH13d4WGg6gEmzMwamRIOhjHvrEmnP3t8FfBk90x6eBcnBQ6l4w73dicDtT%2Fnr49Y96MrYUjm7Uyo8Lq0uAZ44UG%2Be9ZIouhW9cqNE0OktxTUVrZ5aqdwSzeRKNukXA83aCnrx51Kigt5rMbwPsnhwOhrdvzpRVBhTjt2WwyGTyQU016p7ThLo8WU0IzsV79RiZlvABjhSsOcO4vZM%2BZ2MMKj%2B5cgGOqUBT2uZhBFImP6C2HfHz03hLtaHm2l2BMEY502gPTlBzhAvMBBZ5opRR%2BWRK76jrjf9GCWrHajEYsGJLeLwtsOCSbamiFnEV1LfpKNWpsI4YH1mzGvUewJWEXX07Jo6xiCMeEfDnnFRhM7ToQj9QKvhX3GY9ES30zw77sZ0JmK%2B2ZdNfJK4NBVQYuclAhr5lITYlsmbyN4kzPLUFnSejpEWsy4cQ4KU&X-Amz-Signature=63e4988d97ce2847a711a0bee8b34b89c41832f7398880a3a395ad62014fe37d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


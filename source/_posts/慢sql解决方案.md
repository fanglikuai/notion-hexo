---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4NGNP3%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCGYrd1NXUdl%2BID11HVzbYrE4Dr5bCbcCYv%2FT9JyGqavwIhAM7Dcg14zMfQG%2B77EBVdCtFuzBeACsLQ8r6zWqs715kLKv8DCGoQABoMNjM3NDIzMTgzODA1Igyql6yfKXXSFv1dWs4q3APS2sanrRIbVSngodVhDllkUnLDbep1Jg7ccdJ4BKbcdbP3zW1kSj%2FJD%2BogJmaFmjs%2FMyxQ3GXnigsEDBno1I8akkHn4WOxtvJ5IC1olDQwHRkfF%2FFpHH7jrZ8%2Fm4hT5aSgIn7%2BG7LEprfaZyg4%2Be6g60I7FjpCRTKsvoUuWm3wkwiorkNwLcOBcUpqb1p63u%2FzPOKAQKaT1D6EPIWFn6NqiKZlLQ7hAKmMZSKZgUc84lYaJivS0svhRbJfFJnt5OyLb%2FxacJszum5WHJtArTziASacOo4ap4PDhSsmlbH%2FKRBMHE2QLk6pyQZdxoGXd3nzLW37UiT9797QpWppamPFgjgMtxyEgljFxuIAmcIIo5AQyoxtinR0iKRB%2BNZ%2Fd2w1aNhe9JNEOiC152Xnd%2FXsAmG%2BPJagtSRAuDNNWiM6G%2FzrgZlIV0cbAWliLuWUoeJ7hdxff5oOPs6BXHxS5x0j6nQXu5B52%2FixO9IIX3xkuWl0zvhYL8kp9WcFPRRXEJfFmtuSEtUVxAx%2BMtvudzMYiref6huIw65%2FSYLvSq0yRbkiREXRdeRQPuFPlx7LCnmzdQd7GmCmHr%2BFp7etsAPFi2eJhtYqFr3uFfHw%2BJnvoSnCNKuN8UgB1xYGlDCrnaXIBjqkAR6wgBa7hR1ZvwrhZznGaHjPLIB%2BwOgU9%2BMDIUV5eryIiM4niWpN8bvAxRz9ZkNSvVPtb6R4vSn%2BfJ78vh15D4w8g5ToXjN%2FDhJ9fQjssTrExVLiSGcP3gRIkK3dzhdRnhV%2BVZA6sZJrINzoCNvB8kmWnnLnCQV7OU%2BwoxnvrJPZm3NPfnnvZz1o0aVFWqayqRvXg9yeLt88LfuohBOcR9E%2BP1bw&X-Amz-Signature=a16aa3fceb1e2aff6e8e9f558e102163f8e217922bcc56c84221bb909c8a2d40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


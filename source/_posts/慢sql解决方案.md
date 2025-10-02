---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZPYGU2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPDrF6iaJfWEY8YhPQ1%2FnHUNZp9eW5gGjRfrXG8e5qCwIhAM9AYyyP7NdfhD3eXgO5rJD%2FOTzJbK6CL%2B%2BqdJI9z5ixKv8DCC0QABoMNjM3NDIzMTgzODA1IgwCvuWMr77dBdSsgmcq3AO21vs87dfYmpWaakLSeaY67u0j73ihpxqVvrq7QU4Dd3FbH2txVVHI6ZK8uxWrqMAVpBHPUEw6mx7itfCZwbtPmE%2Bqjsl%2BF18trhIGuf%2FbchAysxeu6oN5Csc3NnVJkpuTpjeDgGQCvk9ty51tBPfg0BRubabJJyKqY%2BOoQRLGGfXBll3G6hkuqOQnl%2FA2yNZF7Mc37Q6xCdcvbjr612YP9%2B9FI6qJfGm0CUw5DqrNwuSzoLyVRp7a4Yw5jt5Vk6rdvQPjlnchqIrynbv0N1NHpMUbk5iQFhmacY4OexDolchAGtylgkjc%2FUdDOwEKUnho%2BQ9N9xAxuQjcr9KlrMk4WLrqQ%2Bc2xHJHv%2BKFwQPwY%2BkNNO2hxTbX1jlPcJzw%2BWe9I1renwCBbO%2BJMOTxWLJ1egmeZMp4JiFGw6Tq6IubywCOeGCsw70Jvq1pU0F8fGRrPf%2F%2Fo9Szkly2Gp84gUzYXKwOoc8EV9qJ%2FyD1z89Gr3Z5mQHt5DWuhKNXfvvgmBEXCbtGHZfL99nP0KHs04fYPJDCPTUG1Y0FUZvtfBnXEs8xjT3bZAu1Q6wOP7HZtfGXN5NfGB4VS5PS9%2BkaMPYPYaUD%2Ft8LMcHnjcMgpGK9xdiCCCfA%2Buw6fI%2FZojC1w%2FnGBjqkAdiyzjDnZmwsUopvRvAobR1kK5kKLnMugbgHCBf30M6RGSOefa%2BIE4QHYYysjiRuzlv1qUw0oF7oEGOWGGNMpFTD%2FNP3coLSJ31b%2FXPy01b5A5gzAuIBWwLTN2%2FiZolOxbR6R1a054twaPUPqaEv8fHVjxPrZoXuO3ur7iFkeTs5VPKYilO3Yx%2B3%2BB%2Bd8WTaQqLCi1mrNDflUhcCranWwbpHM96U&X-Amz-Signature=905fc7d86fd7a77656ece4cbf0ed4b48012ea33d70fcec3bd9d26f182de3b5ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


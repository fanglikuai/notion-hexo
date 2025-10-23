---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCQJW7NA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBKDXahPa2hzvGonSdwAxPWSnCjYIC6Vw2IeC4qkweCYAiBH1TXuHVtiPS8p%2B2oUg7tRfBdtOWyQlVGduUfNs0YnISr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIMYaOF7yRB8ELLqeWbKtwDzxPxUB1i2z0sDGl88taK4Sm%2BRyq0ZNjKGWcpNT3s5VOpxhXV1q91TjzC307UK6kb%2BFE%2FzxcCEEvI%2F3CKGh76%2BukJjdCRdfV95P91QW9P22D4cdlyZLWwWHjDIayTIoBYBt8ePsrfTscoTyBNP4n6VElumeKuCFJjPfZhITfBV9DlcgWro7iJvCd1pSteW0DwCaiP6YTpn1pqif%2FUxxFpnGNBkTwdHIvdVSI7GlRMNyttCvpQ1vAxyqWpm0LMWqR9uyYhWPsKdDXRejo9kFG0inHfwYz0B%2B%2BlI25asLukdYucqaRO%2Bea5dZ4jCvRssOGzjHAg0F3sB%2BsAnFHF9ZOByTq%2FC%2BgNCjojAF3pHf2WzejrKGi%2BElSdf73Dg%2FPc%2BlIM%2BL0VqFASZ9PgN6xU1aDuT6kdu9Hjyb6R8VYUehBYbw83o%2Fm4MPHfwnTFDQeVCVbhCVT8YzdIXIasTC1JNRftGBRN5Cu70NpLoCZ8dz9CCW5yNN1Vyj0YWUzNHuRNFuK22ArS%2FjYLQakiZp1K311FCRpeo%2BfJvQcyjBbuvxK9oK%2BUbc7ap3t5zOUc5ycRw4rQbGaVXWGoTVSy9SP42c9wXL3WlZqvUzRcKWR1hgiq4BFU4fQP7Kpp8Y1Hwlcw66XqxwY6pgHIHFeVMcBmsHu6gTAK9YMEM6bzFkuYRedIiueNwCXx8f4co2o7CQ0e%2FOzIXYv0Ca3gmvANBk1pSL%2BeyQ6JV3H%2FeXBPooHL3EKtONrGbaLSF4SZ%2BAgeSnDcQxTVkLOt4sc7nUs5E%2BQeHv0Zffppj25k2f7mwmrC8EPYOKTSWmyPQBIoRxHYhrmJnMAbXcLX6rtGn3c%2FPba0oAf8JRXEIQ9gtCwW976L&X-Amz-Signature=ad2d94a474c8fe3a09854cba6ea6ced9e7d1b9560af9df756bcc56c1aa29eea5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


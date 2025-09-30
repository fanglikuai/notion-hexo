---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Z7NKKGY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIGBvRQTFT2e2nv2Aiefz8EBwLFNzsG8n9mTobK4msEq4AiEA7B4p8PrnETzR%2F%2Bq3K1dQKGpHTfz7ZtybCskWBcfd4DUqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKYJP8BSu%2F0e9uzXCrcA9iMdH%2FCFk7YL8pmVpD%2FudfIo%2Fe3gbT9bOI4yInPi1oJoiSAHg0RDCc95cuORd58qRs7NFtCnofqIfKofvRKvwcNfqmfcbdu6%2FTzi1DGDr5A76aVfD72AUiW4WO3K%2ByNXoroy6argxMVQt5CKj43R7%2BB4ww%2B8ThYEUTFMz%2BQlxKYtFmdsb1a%2FRrDUvhMZ%2FB8D5QbLEfTHBEkir53fX7I1LaiteduTR4ee5EHWp4Av1GodD7pC7BvKsxcH2ajJ414R%2F01BDpYmyOw%2Bh9uKfu%2FJZHFn%2BIQ7HvfbDDKuJLe5RHt7uv8g2zJZiA%2Bhw%2BBdygLLYTr5KZpzy4R0anZPglRmr0eKWutBwf%2BT7sc8leJHja4iR6nB%2B4BqO7akA5JEuauZ%2Bl72sZtaZ7wjAQSOjk5HzGSd0wjMeT0zx7ydubV9yzwXv7ylyl4AN%2B1pGS2nqKn7Q1DsdmUnzH7Mfj39M0FrYgrx8u9wLSVKfXpVrNkL%2FgsKB0RnH%2BheLhbyRF4XsE768zFixp6hTuIrbUynHFa1WaJCemr8NVGqiXDTPKQ382svAf1%2BOxl1drJ3cz5eieqPRz2W02FnIC0FCh64yKNoMZgIr%2FrjAHvFXBc5AU7B%2FIfpK3RGQUJ5MBRj0z1MMbY8MYGOqUBZjSeTDJImOJ6gHeXHChAntw2DCPwUL2%2BYq02GOkb7uYxp2uEcVJs042gRbeaZzMeZZlwnZk3XiVfvCDaMv11GVzTpB8mob9aLxwVCi7omeLMf8y7WGInYaGtN5KyJ9u1mm3vczROJBPXfgFfH4F5U6loq1dUohZte1BCs%2BxWPH80c3d2W%2BOIEAQRgCKtSvgIDDRVbmrPkKNV4xVxCPByo6J1t%2BRR&X-Amz-Signature=fcfaefc28653068b67e9e36e68acd3bd79ebcab9fdd849221b1893c051ac9216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


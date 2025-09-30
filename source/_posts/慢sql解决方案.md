---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KKU2XKV%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJIMEYCIQC3FSfc9FqPi7eDUNh7hZWsSP6TcOPsAIboaaiKc%2BLyogIhANat2kpTdq%2F22ePWZ08nWfcRwZCoTySX%2FK51Q%2FE0bq3fKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRYxcfRjWzn2Aw8zkq3AMgEno%2BtGZPPLNNxiD%2Bi5tfbMHrVkHvW8qqWWOUTehkUnsW%2Bhu2Mj%2FJXoPARRfzp6rjIVcJvJAXJTzalcHP52%2Bz%2F5w5DcFtdMwBQSGEMsbrz6Opksp55ehYZU2Yw8ei4seE3TRoSjPlVphLAP0iIhZ69V8EaOanR21DSRbK61hfWU%2F30vCgxYVuXBdtNzhI9DXDOEY%2FCqcNukoRSuoNfPNuI5wUAr4re7r9M6vbiG43J2wLT7ytXtJ%2FuU2zLzQBzAhZqWp962J6Po6%2FNoxie%2F2msKlbsaIGBKqyvl2cDHkloJ3hYV6BFGipXDv4zT%2FLVPEi9VGAKfMynRUQsxZmHG2XOic2Eu9WLeob6Z%2B7%2BswTRLrnH6WVeEuMzmLb7fHKtoJrsLemCeeqh8JtcDpUzoz9yEYf%2FM%2FYTY86hLVmVaBL8R%2BEXueQK0fDgsBbEcJYm6bb%2B9vX%2BZ8LiEY7VZslKDe3pdxKJ058csPfmXESmNPKw4ctY1BHxtjiroxE8rHSWi%2Fz3IOPYX6UxrosQZrpvH7K5sjFxIl%2BK%2FaJMGb7IBeR%2FgArUrq7tIf49UstnpfDIktR3b2%2ByNV4ARU46xyBdcA0Czx4Y0pU5Kkgkmmn54b9osqF3rTtixg3Xc%2BWwTDVnvHGBjqkAcvh4tXIjikCeoE53jgX7nKPr%2BRHFHEKOWx04t%2BjFIXxiw92teR%2FDF1qrlodhOvJkRfpNRG5Yk8Vhbzf42XdpAqFp7OUljxuvJXEbtsrg%2B6vWXXwFhDKX9NR0SKwUUfKXpYrRPL2djbN2Kvk2geDJEZGfm8Kr65JGHHUiRi%2BzreYfPQcjPqmT8JGaQ6e09YG0C%2F7ulkm4FdGzVLS0RDYHTZ2CRCc&X-Amz-Signature=b2cd83961930bd72cdd4996f78717ddf6a82b0233d62998d9763d72000c1b1f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


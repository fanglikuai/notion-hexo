---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIQUDSJW%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRKxi75tXMheJrLS0QKbpLpdfe375%2BAWQcz2FHDl0iGQIhAOfc4vGQKpGByqR0XlaMk%2F07pIh7ZNNWLouHbO8prJKMKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwy2Gb38awfSTtkPbkq3APu6mlkkIlM27sIMh0GZ8I5tlLgGh7C5aJ86dqfCpXS06wpPpmjlj5chk1SrRxFq9CI04kOfhMkWWvl8K68jIpxqjd4or1Jpk2j7F27G23OFdp5fgD6x0AMUoFsmEbkQdY%2Fk7Ta1H7UNKLYh5z%2FVR7D12K2qLGiWs6m3EOWHKtGxnLe6IkRhydehh3Dflf4PpMeEcbLt2zTv9j2ZxoiYX%2F3Fu7yS3x5DRXQzBFsJum6RWOBB5MmrmBtN5TL%2Fgy45iIih2YUOy7zCjJA5IJErDM9T1ZN%2BR4C1wfzfhFRG1XJDGy2oA3fFSHDY%2FTATwPtN8TPyusiR1yI6Iu%2FZ1Y50EoYn46If051QcK0xHEk6kV6xLoKPwEJgwfQkmReJicuzBS2zLkgm4GT9UdHIxoolVkqPqwsc2Z1Kx79%2BOyCLN6O6pghe76dieMw%2BRD%2FxxFvVsLu5avdfKqWftVzSLe4%2F%2Bh57wZ4AAuH03LjV%2BvGZOGhtdBxDPiFHVvMQjYobTIDCCGBJu3mbDG7%2F18eAmf3GuMG3bHQwm0kcnRX0Y7J6rJAS9UphUr2803u8byNddN%2BYPYPzlTUTYv6XYWdMgwjlwDLGlEgaVRFpCVbV5G4rafFCg18uABi4gsXURKtXDCw8%2FzHBjqkAaIBMDuZDwPX74D2y5qbkLXNHId45E%2BK1%2FV7SLymiZBB3hFlO2es8FGyLyiEAAoXfdGZe1CjDQ%2FKUnr0qHtdAMdW3FH2%2BhHxtWNH8VS25R%2BFD4jOnClG6VkUSyMusHJLXHGPFA3P%2B8SJljhlVNv00Vs%2FgD%2F0DZrYpp78PA7o%2FoTSXLdTWEvwgzWGZPaHXwCccQdFVQ4YXddbbF9t%2BGpQ8zM%2BbX%2Bv&X-Amz-Signature=79383589b15f688c4cdd4a6a43da6f782c7cb3a3b6a2041f1b3fd82e8c229c48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


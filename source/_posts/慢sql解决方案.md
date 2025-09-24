---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWPNB4J%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUt9UNkmlyNKeC3evLyJdtd9LKVhgaRPW255ni%2B%2B6xEwIgLPJzQ8sLzp0cKdWaIECl%2F%2BdBTN%2Fvr1v%2BxWYL0jSk8%2B4q%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDII8K5N%2FQmcDqyJ%2F%2BSrcA59GxKsMVisWpNUJmnTa2fyWa3PbY5JVeZ%2B%2B9w3FUCCV0rZCyCGJ0ea9KAm8agpAiQCOkHwWKoKWtPf8h7mfgMZEagQUdkWUzTVrO06QdAsi6lOwO%2FSXO19eQn%2BpWI95V8cHx%2B1i3%2Fyb%2F4CXH0lyH8QaJiS404EUEGyaFHU82d%2FLquc1pCan414SBWV0tpBQqt7CKfxR%2Bh5db91ejXs%2B7kckifSuUHmmSm4qQy%2FiwMHc3nxK3dZCRGLHJYdLWjg1QqJMlts080m3RCxFsjGQ9MX97vlAOjVZhsRgS%2FsdcXycy3qr44bfyofasi5j7ztnGgjHfpWd6NxPhytBXqSrmPXxfsl06myNB%2FpwqxYKEpAsi5tQX5NFyndT%2F0%2BZaiwoGil1OoempS%2F%2FalFjEJ73Wm6R8hvOr384D%2F8Ps9SmYlSkDCv2N7TE2DxPj5epIrDESUAHOSdUK8kiGbqSXfBBWLQ%2BtkHwdcu0cpfApG0XMfJIgQSRUXc9vBKpA5eA5oUhLbPxsI9U5%2BPD2H9BFfN9RUJgThTw39SVHIj8vrq%2B7x7FCiGF06Eo2bW968BJg7iWayUzeDhoyUUtkxZP2zk3aMfxOv8O73EBoKXvKD6MgjpNM6SgqZWTNA%2FeYKzrMP%2Bd0cYGOqUBMoyQ%2B4Y6YUKZfPGmw4wD9cDjggFchB%2F5SEWtVidSC7moo4me8jUt4FUS%2BJKTBY8nfsSU0ph9r5t0RONh7KuMoVPlIz7ojQkaOfg%2FA%2FttbUH0jW2FbJ2Q6Se42jy8AZqfYvPvCofkkTXwJpY55MKGZgIU%2F2mghfFmXdX%2FFDybuVvisDh5A%2B4t7xAQPvEg49Y6RsDiXWPGAk66o8uH26e0f%2FZRDS91&X-Amz-Signature=2b6a5b6ba3dd11dd005ffacb7996da2e5511d5bc39bd54c89951fd2e0ae36de9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


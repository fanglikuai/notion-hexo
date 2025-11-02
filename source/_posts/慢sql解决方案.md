---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBJAR3GL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCYK8b0ddejPZ2YM9f4%2BdCzqL8sZUQy8tZGYzaewGECYgIgSvYojzrhtKvHW%2FMJHjY6LL53z%2FtwS1%2Be5N8nyxt0L2wq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOlcvxg0%2BfcTCS5uoSrcA0ra7OUjr7vTpGra85b%2FPgMYb6V4Zqxx7rFhAxnsR6LrOZb%2BqsNCghgY%2Bqfg9RSdsf4cDs%2FaYSNHNOV%2FPdSEHjBJ7rY68R6xdPbtE%2Bl2BoxiTCijQW9FAD6PWKowPSicVNT2itrbPA3pPDe1e2%2FpPjQVxAE1AXi5ZybB8cWOQc%2FWz%2BMSFRWcKt7zeIbqUWn1Ik4fzbvznZHKx9YTRH6vuxnYUZVsyD%2B6hqGDuG2eRdZbsn%2FX8Wrz%2BjEkuMio8C3m4%2FdQOpsJhFKpJy0T5jVdJ3%2BRMd4CbzvB0RtUSinxSoOc%2BsE6N%2F6jHe7kVEf%2BJHu5eawR2DSpKN4%2B%2BlzU56kp3VUwibk%2F1nTrXOWlMGTsOIXiCjVfxP3Aq2upNvySQNJMNJdiZEZbqwNQhkpY34rB%2Bfxsy1U8XwsY4OXMgWEYhNdsWnAXEpG0vkkNXbpyOZwNfNdzZXCTAusgCmdsW%2FTSwzDfp3Wa6pFK7UrL3kF1lZAN5cUAC137oEi5mDKEcLis8Y0QuhPzNgnp3dsZ9eHbc4Y3FbgqtQFna3jBH7agpU3vR6f%2BHFTsDkN16yXoW5HnWPb9BeGp31zp0V02JKQZYN4QsFbcyupTBCJrdV0mTI6l29QVP%2FrbPtxH%2B%2B9dMLnUm8gGOqUB%2BH19rBrsLC1anjaNKkTOwW78gap7rS9EPcQbZQjpv3k20l%2Bm%2FjdB5OldHKz2274KoktyUGfM4MMFSN%2FZQTX5hlYXTdKhYCfC24%2BAkljp%2BbLd0GkE4PjS6vDWYp8t2KMSo3F4n8f5F9PJDXbL%2F7%2F%2FbJSFE0B%2Bm%2FcY0lVaH80uP%2FjRGNJF%2BJDcyWtEyxGEOdGcPNf7429EtZbBnvQ9yYQ%2BcoBTKM60&X-Amz-Signature=780e2ed2195cd018243eeee64b59040c561c92e08a01b4b3756bb9ba9b814bcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


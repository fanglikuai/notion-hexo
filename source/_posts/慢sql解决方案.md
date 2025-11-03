---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5BAI7UB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW239Wj97oH0RVlV7hHZg75VKLVCcnOIQOKMvnCPAwIQIhALFEu%2B4VOCICLRZvE7svFwxgX5ei2TrDKMC%2Fis3iRKzIKv8DCGIQABoMNjM3NDIzMTgzODA1IgzxWMWDQtI1Dj%2BGVKYq3AM5WS0K0Hfh8J372dbQgGAc6FuaGDh7wMnEJg34pwVLh0khHHq7O%2F8PCHVo7RMHHk462DcKg%2BKKTlI21QcevMiqbTnH9L4rEd%2BuajF9GY%2FZcOcqmhKGk48NznjopqAXT5TEb%2FxIhnbilQXsJnIjZJBgxRRjShCFjDGdcXaWk0QE1V1gTtZ%2Bx644sPs6DHvRPqtG3BEjOhOYHOxlzPdGXvdA1fcA7Z7xr6gU30ka2kaIOpk%2F1Di5HryxCdA1CUN96V2GQ4kwHNj9%2BP2x6PfWYw0hAFd%2FnaDkM6fxIAgH74K8PKyyY9YTsO%2BeBea9qJnVAEmrcLVTokKqS22CEsRogcRqO3G94FLtAoc5na5HAjw%2FDKJMzROqNFEuf%2BhgOp2TpWBaNIdbENFg%2FnvloyB6yNNYiftsIFxxEyflpA0dKUK8SvcDyHcl%2BEf3XY8DLlfmkuUhv5pXYemmP7gnrd35sFxXy3pm6BZfaFLDtRBXGCHdOuGIGuuLXHtfgBOEl1tWsTHs0lHfKZB6FYBLbjqwhvhtzH7pXKC5TiXf5FkbDK1t1XsxUkE%2BbM21wp%2BOibuURNjcCOKDYbNDbfMFWavd%2FL31q8pRwWp8a1Mt%2Fr2POcW%2FZd%2Fqf%2BsBPPWKJ4ZTnDDTs6PIBjqkAUPqFXhdLj3%2F1gDJvv0yzbfX2l1DpJy%2BAf4BerF0qNWEV1n30U9dgIWKQfY1sRGj7mDqao2CKLf6cKhT5YDTLQus1MPXpGMQveu8YzqiXjnM7JzArhC4yYTqcvsUN4zWB3gU%2BjI5uNriQ84wDlK0awYP9fa8xRlZyKQGOTPM7pdES1jXSbc0trdTWyioognZdZ1cscqlWNUQN7%2Fba%2F7sZ93SsGNo&X-Amz-Signature=3c3cd113c9df9bceba6d849396fbcd33f64b6d4d5fa117bac73da3a21a6be8da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


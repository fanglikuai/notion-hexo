---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663L6PIL4I%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCC1haMb8PSYZy7zVmRu3PdZlNmuofpwAYk2vI7C1zLNgIhALu6jOu80jMCIFhHDLBETJsaur4nePtIMefJFJFOZATeKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDyCHanGKcPeQ2g60q3AOgatAj%2FEQSOVDxSdEwE3heet7%2FBQ82P%2FW1PzCzBrjDERTzWtY87CKci0Lx7LHQPKurLf7Y6lIITVlS%2FUpNUhjGbAwYhIe%2FUvr83oO7N3d8is%2FxPA2e%2FWxuIJY7GXYo56YXLs8Gj%2BqXNbYD6x0EKWiu%2BKyilRtu%2F5yx7aIvl%2FuGf04k8WTHJsn4lmXqPGg%2FWj5%2F%2FtTnJjjbK1E1Wc5McFjfuuzqiG09XJ6DNKe1B08cMzIiiKt1ChaduKCwWdQfT8ElnNVWJJWVbzGGym7Q2uFkm4rM%2BDL13weuRqE5DT6qacry23rGlBdKd2S0Z9QxesJ5qjm7qxgEqTwhA8xNWdORYb0nZpJ1FvH5ilFEn7HAdrv4agRq19AfAFFbLlmleay76Juuk8FxK%2BHX1vV2b4dQsPT9l4hOWFbpBt1%2FYOsjZIzgcvQBby36ae0oZw%2BN2f9L7l91GCWAJaDJLpOqBcgUDvgeFu2KQHds%2B51%2Fy7WPG1xhr4ojNrZWrjckaUEGxyX6ANYAg%2FRsoWPUcrnP1gwfYf4YA%2F95FqYZvWj0EYJ46xA6xlwvJFifDJdYJ2PHppRcb37bQKDxMHz%2BRCRVXEGFTFAHJnhNJg0PG6PFbLXwlUH1VOtxvcbfZwu6ajDgsI7HBjqkAaIksmO27wEPUQa8fgXL1gyM91hKJ8uKRddobYZsGZYhmRJktdLw7GMMch6fsgMaitHDJgiElQaRJ6P%2B6cuxzRXgGE%2Fyb15hTictBfJWyaqiBX%2BARUDiDSkSxcofIy%2BL3wp0iAsfCoNhRjeKuRsChDpEzWhd7VnZ8%2BRx%2BQRzOoGm2MYt2RZ0c98bzYbs2AJaawTirsj2a4lu95uGAtAGhcAcUkY3&X-Amz-Signature=9b96ba91a21a068052cd0b6fccca693f2d686dfd5645433e9037bd7fab87d7c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


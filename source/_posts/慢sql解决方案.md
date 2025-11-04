---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUEJP525%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIACWTek9%2BjOuX2CgoZtXSLqSg%2Fl5DtJNzCM4z6c3D9w0AiA6j3KgUWYCCQlssDCtdTNG8yuOGTH0yRGPzrfKO8ZzPir%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMndUqGPujPjgATe4vKtwDvLClWVzgw7tU0sD3Cs9NQE9zUEjPEH7h2riWXmP%2B4%2FOzpcG4yMTEHcNA%2FuSBKFcaFeZTl3I37Ljwo%2B3LHq33tz2zSvv14%2F1jrweQUxrk5NJpPc2ra4FC2ASecDM2xbeQ%2FyAJN46PzFSwSkJyFkOfvKnAwo7QuSn2aXsuW8aCR%2BWFjnRBmikhwp7cl8%2FpTtC0f4UcmBlrU2IJGuDLuyias2GBqrMZxG5riBEEz4UAD6EmI2%2FGk9It5DaW4jqokenS%2F8OZOZsZzBjBvau%2FOA2%2BxArY1QCRA2kTvfvcVTg9orpbspgKjTnHZuQef3Z9DiiIP6Y92eZb7%2FsXMGPvr6fsvY8jVLH8Vl6W4k5cWX%2BwG0tHsGPy%2Fwsz8haRady1TTdqjtOWvM9trWGwVD5vJi83RRxgfoj%2BA5ThWWz%2BHa5YOJ5UHsyX71onrJhC7ATbDHwjADinsDHgMVF4DD62XJy%2BNDFPb8t37OasWR1gqlqy0C20PVUIg1P0w1MVQfcltrP9zldiejia5DGeFhNEyBfuQakfv0MFSeO0aXjZFZ4APFFVOnSj3ou5wzuIFcQVD%2F8hqfYzRm2yJyVofW2k%2FGdaelEISWnEt7ObdvxvRhCuRePq0JF2sjQRjIDUESQwwLmoyAY6pgFJz%2BTp1CkZVO8JTTyXg6TwVOQ5ZG4AfCWk3YGNLVTQf8907c%2BnSbVGIcar9Ab5KBIjv%2F7xidALWXfN3CUkzGkOCUv%2BoWZQPvDtezO4sA1kIhqHYgYzy7wxmu1EoU2wbVF%2Bom75YyXYp3Vp6w4vdIRSl1AwrkM08v1o0eSu5wa0wTdpTcqMGJy9lQzAwtqIKXD3zTxED2iZyGkMJ%2B%2FCdwbSSHkCvrda&X-Amz-Signature=e828ae3dfe970c1e1f7af42fb977b70b420b2478a12857a2299c35b1dd215dc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


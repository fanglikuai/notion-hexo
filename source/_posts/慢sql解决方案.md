---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7R3A6QW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T170105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHjlkqMi7xefwZ6yvzuRh2oBvnpyP%2Bdj3YVbsQp7QFVAIgXW2Vd93eZdx%2B%2B5SjlJOpapndpAzc%2FGEztxIszFb6ssUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkmGqOm7aVt%2BQFASSrcA2odl6D%2FMxxfYLDwZBTvHs%2F7PZRnfeoOKhDCgT8656vygDSP8qII2%2F%2FOMIqxO8Kfcn3gvxm6T5hfyziEpY12ZRGKeScm6pQ6ttsndzOIWI5TKVGFjcnP11HUFFe%2BVlLMjRGwdCamh%2FvOS0gpe3lB87HHYq4xrFazAGFZJbwCgP8uEMhjESYsFuI%2BYU852MPs2tAQqoONxDG4Yod41X%2FkIhfzxmVeHOhQFU7n7y6KVHBf%2FM2Vzw4aFv7O6P6NUYoFZ57spEeUlo81nips42zGogfHU8gPquf3mQALeXaUkWnZwU4kVkgM7d3MfPqU9tgqPlU%2Bwao2x4ajmSDXtfcdNv9dRoh2IJSbvnKZSjkjphCspX2mjl%2BgTt07Wug15rIwTwRdThe20iDggIpt0%2Bu3eT%2FzLac1aNEnRbHBWvWHbFiHq6ioO6x3%2F87UpAd5uiFjHb%2FWcczpGIJ8uNtMTcVKyw8Keb%2Fs3nHX1BagAuA8L8sViL3ulp10BCgTDVWbidAiWnMwWr%2BziemeHeqaUrOlgaWcAUaU2ECzg9qdJqhl3dXdvivX7PuWTn%2FgXvANwA8I3b5QeY1pQQinK1GGOPflt6j%2BqLSraHF5MGCzMWkNHWuresBjQHL5%2BK%2B9rLFxMJfAn8cGOqUBJhaknO0I610B9jMv7IeUzN1lntUB2qHD39OXdvwlQBb1EdmvMikUd0CTUgfLAdvxPdcXda64jDyne%2FBArAiC0tDFAn5nx3fmSRvCDGYhWQnNcZyFyOMhBbJaNFUe67DAhJVBlQMtTwGLqaRiLeT5mrXaTo0vtg4CXfQeMGKxzxrzAxptF2npj9z%2FnhJiD3T30%2BYup4wmGk4dX%2Fc0csQoQqmYFopN&X-Amz-Signature=2e8c3ba9e3d7d08e10a56cda9eaff654a5a31827aaf3deb41a61bbad63876ad8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


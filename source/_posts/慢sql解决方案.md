---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWLNDOQW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhr%2FihyR9V%2BAWI%2FrsPBPL1Nro0HRQZKx8dpVQK%2B5AqfAiEAmZGa0ZgQu04AvhQVC7vIN4mVR8Lpyf%2BDPHK25YjQw%2FYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN7lTFOV%2FqsfBPyySSrcA6WTHhEtv2GtBLT74AEJbK%2BopF65TwJac53pn5tP%2F1YV2OCJ0BbEaiq%2BIdfq3SDeugHywZ9HMWshQw8OCo1JNsUFIM1C6o%2BL8RdGwl7wwU8HHUsrCrL7B1VJfbKzH9pPhdtKDR5ED3DJgaSRWKHv5WfOBCRbMnhVdmQd901hWo668yBu3QRE1zJmdiPMt3CcZuxy5deaPRnAGAZzyX9fMWokzz%2B%2FigJkETHJUjmHAfHsX0qsQlgCoMMQU1980yWqv4UKRGJyCeFWHdzzwgocztN6O1fAy91%2BHnK7JL%2BtsCxgUmC2yF7KSdZrssxD%2BJcpc6RbUfjCdF8npebCfGGXmmbypeJ%2FYNbkqbfbMWchEFRSu4yXaZwSQi23NVhmBoh9dFNX8yyV2LtEhHnuyCgSei2oCWL%2F%2BsKtoD0CkgP2iTh5Sn3nlMcsPREU1G9o5jxi4%2FxChv6KnSEF8onOiOYQJrJyNk1gk4tTfuEoGu8268cHrrGmw0zMM7T%2BwtIktQd10sueXkqe3rD7aBSgr6qmYiqUmN9YQgSY4a6FZ505jzm50bBYdw%2FtEk9UPXlq2%2BY7XIrS3Gb1w90s6Sl4GOy7P%2BwxRhcRoVXAHOhE3qrfHZ8IZBLblwiWq4ld6ktoML7HnMkGOqUBNSidkjR3YV8weS3%2BtY4Dyk86lFVtlZZFDdEtXhWlzclOg3IWVWuHhclOdRVjJu214SCkavC9rrw5u1zW9zptyXoJdMmPL3jGRfo2aXvBhvUqFjhnMj7YaPGK7eECSb6JNc2RhjianrZuWGyRFBOtD4%2FGWhqAzwS%2FblM%2FePl5Br7v2jA7nqTwOcIOH4X%2BLRWx4HNnCdPC54mZKgctXM4Ke8ze7qa%2F&X-Amz-Signature=30e05b72c28bde2b72250f820af42f06c51e155fb31b5e7a4e2c6c01531a4e75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


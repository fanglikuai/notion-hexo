---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFI2FRNY%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHeMCgyCzu4KeXcGEtA5kT6mkqN%2BXcDxkWPs9sQAI7YvAiEA9t5z1oFQjNzuecX%2B4JhY4SmbRk26nuBYKfFMc43SdvQq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDDrDaPi3DOLfSKGUDSrcA2NRigtB4oa%2FHy14I6Kd01uOtv1nrq6zHtj5ECqUAEVkGwBbYyMN73J2Z0pQV1AVLGqbuNckIzmQuRKeaWUPyNvleJCaurPPOECqKIYchU2Ftr6uH7NsIK1IZfs753aUuqZ6QKR1ieK1TUK%2B1SKOqpGz37Oq%2F0y17ooGJMirwoJGDBC8RLCf68VMfimYys2%2FoXP5OGdXWFjWDwrBKzOPt5auA7ZJtquS%2FqZxCNnw1CIxeB4hN%2Bn4lSkFBLVNjEogC8G9xpNB%2Br8%2BQyQo7M2%2FEFMqbtV7IGH5PjhPdSpTV%2BdVDCKjeEsODOsyxT8krmCPX%2BDnyxU1UjwDK2%2B2O1U7oz2HNoScY8eKrqb4lpdPQmlFzBtDUtccS%2B1Dpyzg9EnoQPInI8yAvQBTYZGyJs06BSAvTzg8loibzM8muKGyR847QfqCLjYZNEyNR9oO04rT8Kk8KNdwy04duL8%2FJmO4oLD4CQ7k0qaOcfNBfFcukCH1pgQq1PPtL8EQiPTbHA8ecGa7THw6gsbvxdjSn4MlqHgvbf%2BgRn4EZ7ggTL9KyIfUWO%2B3wCQ5MKi6LNx2eYKCtD8haSLbQgp3hkA2fB7KkbaL2JvRVlYV7wWJGuu1wEEheHhh5WFZI8x0UQkpMMfQ9cYGOqUB5KqDkH%2BpBTyYG93c0%2ByTRih6BWiPNpt8lRhpH%2F0IlqE5raplPp5a8Eh%2FI6GRCIz6PMnalAkMckQjjVOPQVztYdWUl2QoltjYYJyTPlXhIo0jaT2Qv4XYIJ%2Biv8Ps%2Fp1BUH2rHa9O4KM4yszLyJiosJoB9iWlHMyeMi29XbXyu7XV%2BPC%2FHZ0qX1nVNni%2BUnr8906w0D1yvbV2w4HIPjUY4UOtBFub&X-Amz-Signature=15d0db5314a5067f8d32fa6125bff9fd1b9093824d3c4b18985c1a469d0e793e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


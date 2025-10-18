---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RDPZLEC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIE3G1OLwOOKV8TJlXiwGHovSXhl9ueKqGJLI73dlzkJ9AiEAlFBHR4goH8AVVZb%2Fx0au1RZTE7TqBvBU0EE7HSHx7bcqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAOHFmiWp%2B7PKsevSSrcA9mbMu8%2BIUveVXEAy7udUjBdEOnhqHY8OQH6qTvOX%2FoYEbDh8WF87L1P7hrCoMEcl0k88LCqx2Pi75DZy6RWUWBiWD00XGPCADbM3huPFUHDhr50b2IotrqnJqiiU4X0v9MdMTMWKmuWXjTpg%2FUvvP%2BTNJ9ftgVDFmc%2FG64i%2FQ%2B%2ByVhkhhk9qvl%2Fbc2hLIgpOtXU668RaMuEke6PffhvqcO5HCWAJbNAiMU%2BOdo8nwsyOglbpMxVcue2JL4gD5AHTMJ6sJhzKMef%2BI3j6hL13FQrlzL9LgTDTEnDDb%2F%2Fus%2F93hr9sjYjDo7%2FDty1aQkOCSLn9q94CS31Pd0St734sfYiWAuxEAhdUodxt%2BWyPiu%2FK7leuTkIRc1M20WyftI5TCuqgClg3xH6C%2BbreVb1Gg4SMca%2Bj6p6a5E9KGOB2vmNqoB6RupxALtj7PGfRL8QXDqq8Szw6X8NdtzqL2RlHPJBYM6Agx%2FNPKUI%2FXdhd8YChWGg%2BbJvmDgB9Dk4F%2FB8eVwHcbgT%2Fkxdx9STTPUIwU0mSaCF28PTLrR0JNiNa90ys61H5fEk59uyME0ooCBx1sL3Kzux4L22VK%2F2fw7Q0yMFgTUraVSSZUvaKpDEEXx9JRBtXXHt6gk%2FLncJMI%2FJz8cGOqUBm6DZkzciWF84LRpljRMXUR%2Fqa31cNMdibBy739PEOxI2wEvionnQcFoWJH8UnjihoQklkRrV9PJ3sZe6tdOQ5s2Ao3%2FYcoWnbMILhootlOfulE5xC%2FIw5%2FQDKGtDJ%2FfzFfa33C9fUaUl233TLhPruCOlwEUjYzTuUH3ilXbyHaubCbp2kTBk54m%2BM1wsrOjiD%2FyhjUe8HKM7ZgDbiMo14LlmzuU0&X-Amz-Signature=cacc4495c9d4bf5254f6e2c10ad5382206cc59ac766421b710a38ff8e7ba665c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


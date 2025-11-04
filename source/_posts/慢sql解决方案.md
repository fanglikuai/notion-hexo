---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LYFUVXU%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAMvfnkRrk7AZw%2BMU5yBI44rO%2Fvzj24eYHj4xQql9g3AiBfer15G5Fv5R8NGiubr0fdxYaSOV2oH9EtHtMd2R%2F8LCr%2FAwh%2FEAAaDDYzNzQyMzE4MzgwNSIMV6EVZ%2FQGnEguXt9%2BKtwD5H4jrL9nUaaeUfe8gzeLHqDxfu7Katqu042Bh%2BQo6mHYVImmI31w1KZQjke0lPvAyPFNZciDgvLfg3jV1k4gUtUEtTdMWzalwaehLwLfj%2BJqgD%2Fny5SniBDjRkdwxBuyrNVTpiyPuovgoE36%2FTH6KyVbMZOhbMhYosl2bLziUjBnYZR1P%2BuS6t1KWB%2FBqmWS%2FM1hO%2BB2tSU1Lb4HnO5Ol5sPqHrlqJ4byzJnzR%2F0hotVuDJWCCPW4YU%2FJggguVYbc%2B0O%2FOV6LRVZYzfVxDqD7LutWtxKMP4VQ0qXDMRJkRw1Xad6r08ot5DkH4smrla%2BF86S7B42a%2FwvzK7eWm52vSuVbdftKU0GRPJhvQvsC0bEUdMUTRVJBp7s1%2F%2BAnOlLANYZ9Nos14JhJ7H2%2FhCYJ8ZnEIzkqXJd%2BqZzNm%2B7HUfJ5IkYz%2Ba5%2FI8K5gF4enPuS9r%2FgQ8eWwit2zmSt%2FZAP51vbqEf7NsHTlLTwWTZnvZj%2Bak6HF%2BY%2Bn7pVPoF98xq9M8lNHLTwt9Ygq7ftRyqT%2FHQTbMwy6kEImHpPWQwt5GFL0wMMDGZAeyiEsz0uYwio1kgz434Ig1gHm%2F6uwJWlJZ2jDfVFQEBsF22l9xYUmC9pE2r39oRdfWKGQow6uapyAY6pgFcDmY4OuIoDkxKKlUce9mFSVwjgcxdB%2BFozPtHhuuOPssYPv9vV9IdQuZzJNx11KCqjCs2P30POEawcolQe3o5nQEJbcolvhIaBxTt8oFmk3P0gk4Umi51HD41SRHc3uiFwfJ3hnm3JWdktTk2%2FyweoTmoHTvU%2F8CmMWP2eL4oKBtDWNAq68liD%2Fn2VGgDQHNgGKN0CrbFuRvOynldRg0yWfsdkxLH&X-Amz-Signature=f174d32a74d0053ddea7052fe4d2a9688477eb3eba18991b9f1e413aac05095b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


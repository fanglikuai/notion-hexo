---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLJ74EA%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIDdCzzBO2ib3BP2%2FfoAvZ1fXEuP55iLHG1smz67JoBdwAiAUTkafr0uicsaSTvCXI073EijOSsJ1XxJ0mFck9CPBfCqIBAie%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJU6fefmAVMjMr7nxKtwDEgrUn2w4DSvvCwMf7E1H4OqZCGB8KmwaSOSOzdfym0Wbae24CI0R%2Brjy3wSARkZsvwY2VRdfkj%2BfHkKAIFMNSeqLbUBLFn3zwFBKRHzS%2FBdskI0GeNIYtJLGO%2Bb0jG2r5wnAJB1fAci07V8uv5HBC5ddGZ5el5jlp1eKcJuDTv2qW8K%2FkS%2Bbk7QdcWMPUzHrBdeRP7f3D%2B5CMIZq81Z216%2BGhgqg8jVs%2BZvA68nAZRm5MMp8RwX%2FC9SO5qL1G%2Bj32E0the%2F5%2B6namp2prHVzPziAmQ%2F9vLggD4ioiVKS3Y7zLzk0e332lgHR3UlGV30C1Pd7oxbgVJrygrlKdPb%2BWN7N%2FVJ4rhYr4JJ2jSpu7qX%2By0dkMCyQH2EB%2BlsngGuuOhPZv4%2FVS9IoIvvYuhe%2BzgpwkYYhdvvO%2BWd6GZrzxyuNzjzmYfB0hxjhjbthgR6vctp3vhuOFBDvivSAooYIkyzKcESiJ00rHRQ2fYoVAthsh6%2B%2FUcLc4lHRE%2B0HMC%2BeLLhDpbRwkCWtCaooVKi%2BqdSdvXT2yOE0MlH6vFmtriUauF%2FouWHdYaDpdT7IqkwxnChz7yRtzQFBI%2B6HLyNsX5lM6%2BG1Isa5Isf9Rl7PcvMJJHTP%2BCMIZw6w4HcwpNzdxgY6pgFuLBWqN5wTpqaZuBIVEvkygGuwNdUtq8D3fnB5msR%2F67HX52uimOUPEUBQESy7L3Mecjf5LxZXl6GCOtbctZOEbw4l593aJVZdXU9UF2oPbA352zzjlEtkNenOVzs%2BPTN3Nv6WusWQLxJ0xZ%2Fe33ZTwHfzfpmpeWfzF2l32QtJFrmdkbOmATPw9FmOniAGNFpYxw7IWf%2FJpeWSDJRyod2%2FkXbcV1p1&X-Amz-Signature=c5ba2d9894b39e0ca70679292af33a456f5c4e17590d553570967dedcbb6965d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


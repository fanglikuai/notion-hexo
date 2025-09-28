---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3LVLBNK%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJGMEQCIHX%2FXFSPLdv31jO2mMpRohfKwMBwLBTvf8gN3Viu1DM7AiAaM724xRdpOwBW3paktxPsBnLqFijfiDVATCjLdMH0oiqIBAjH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjseqRFs5Co3jr3oKtwD20SxAhMG%2FKidR06AopcF8zf8VJNEO84w1vrulacry54G1cOcGE%2BRgrpncDjIpPWbWTooId9mCvfxDaD4w8oyldCgGcJwYhArPgcHNgpWKLJ7N3IKw1CkVCIs6NWRcM1iux5k%2BiQ2xtB3Zt4yF%2FieO3Cva5DwdWTLSnZnAxjLum1ReqASShLpDr430YZxBco3vYqs5Seut%2Fui9qtf07eDQbeK9%2BKuClhidbxIGLeBel%2BtvqUoqnrW63F%2FT3fxkBg2LrXx33gZYnYWE9LC9FKYgPed3nEVZW5nQwtq6zhoF%2B%2FhbSxjz6fe1P7nwfUkl0CTUTqCjnReBs0EtOozPTnKmi74O%2FfMsF4NBJxtWZss%2FYNG4%2FLusDDEi6Q1jh4aDcg4J2%2B7RURV3HMTi4ymkFYTZ5xopnf%2B6MEBQI8T%2F4aP%2Fe2747DW%2FD5UtqtOUsCJS8S8q8FVJK9gCsaa376xuYgfuiWqUZUQ0KcHJKxgoWS6b0EzVn4D9%2FXlpvhqMB%2B0urtJU7dkIJzsnKeolg33%2BKfpVIsMohRBCbG6x21RQIfp9bdYMfkBwR9iIyJ4KhQ8VZRUGvq%2FPICGEtH7SUgNaKhR1PR9YCoQT%2BhwuWGM7mKILuPFkW2Q%2B%2B6o7Z7GB5Mw19rmxgY6pgG3kcCyJnMRANXD6xf26xkuDUiY%2FB0F%2B0pCc8TqAUA8%2B6SA4aWBPMv1dd5PYxfoA6IGX9SRuCLrAAD0GiU8x%2BgG1LubCGzfwfEWeoDZ3WXGIMO5fSLMQhsuoS%2Bkzcrv6VZ6gr6Y6C%2B8DaLra1%2BtQi6VFzwdaQFclKDU5A8gkIna%2BSqD%2Bqrw1UzZliNGdwTmpX2eRHU%2F54iuAkN3ZksmXV35osSGn%2FGl&X-Amz-Signature=d0f445cbf6ad074ba917d3ccf5b09d6de116b1807907a9617282e5136be65bf3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


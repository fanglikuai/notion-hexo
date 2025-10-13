---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2F3NRXW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDaeJ3FinEv5EwjcVILmbRixVi5JOsHE2R62pS9VwHqHQIhALdzn6hZmiy%2FfU2Ow%2BtntLaIW8c1SD8WFaKgRv7L11aPKv8DCDwQABoMNjM3NDIzMTgzODA1IgwXdcgjWZuprE0KVsUq3AOjEeCR%2BV9DrVe2nEoGw1bq%2FXoib%2F4qKRdBfntgwg3cZ8bas2eZvL4BODW01Tqe3PKKVT%2Fr0vqVjI%2Fi0ieqSU%2BSvtJyGhXhOs2r47JtEj2jwzoKxQ42g2WAtYRVPzu4tlNetiHHiCb99nH6lM5eWg9IZ4sPlncmHa8h%2BD3T4QrasOLI%2FqMLf55k6NmkH%2BkohBYgHGsmEfT1wMIN4t83ZX%2FdPoxN%2F8f09PgT7PqjUBfRtJOBCta8G%2BCPEMaZK6I0LVHfOdcTYN%2F1XE9Hmeu7cRfcIXX%2FSk5vFLeR%2FpDEtyDxveWX9ozf2h4LqyUKwTMy8NKvBqfjgY0xIqtnt4F4D7hbjo%2BBOtytcHr6Qt1gfWGYf1RijP5BnUdIqXgVSfOqz4HR8Z%2BDZgnasx0LyQ52lOBwHVBOIiydtvtViTX8lrfNgTBI%2FeG1UGz13Dt%2FNT1WRzz%2B4JkgeJWXaXsp1ItliCaNlxl7QUf2jS1WJo2iG9JAHIBzY3E9hBznJyy%2BNoVu5eUnJbMCHjXjYsNj5p5JSPGubN%2Fi4p8pGdC4elzrSFU7dFoKLC7mdw2p5G5F4FoM4IpM5cLhHnemml98XlVMO7LXk%2FQtfkU8PKBM6JLOuXcrUNakT9Xw%2F3dk7kR8PjCg1LHHBjqkAZ4SJnBbpyQ%2FgUDAt6F1pTi3qcHFtXonDCMWeEXSmSKPLzBqCvVLjUkWr9Zimo6BfkdrhcjPoQKeUeMxA2wci3J5KqKHDErUVWHsP9GnRBwu8A5MhSsgCbBl10aMkhPdVTl1Xp204wZ2l8xcUHMpcUcpGufICHg0sWIw7%2Fs%2BkwFpv1iOlDwwqERXbKSEPtrm3ngL3WCk3TLDfkyCO7UyzjCPBVFO&X-Amz-Signature=aeebb68c4316ff613c64ee53295df388c15b3e0efddfa06d2a38ac4475d8952d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


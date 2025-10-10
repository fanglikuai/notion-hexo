---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634PEFAKO%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T220540Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQCWKYU7%2FDT%2FFb4eqyPBzJm8D2MTEkJTizeFCCd%2FklSyMwIhAO0%2B7nlKDYXEpsNJvxG7Mm%2Bq%2FG4fApsRSpJiwfvWA5MSKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwCakJmqU6Nq3grs0kq3ANI8ffHWDBC7eLUzZi7ChcJCUccbkKsEIBMJBYet6Qjw2F6dXAOMJAoLHJ1owOUiuCJaYGXzXaVq708vWXqzNCd8qLqyrsll8Hpz08dqTUJ03g1RdPBOXSYO6YT1OWIYCgRnSgpJpNJptek7daW7Xx4LLAqN%2FXLHQOIgKrPy7IvZsjpn%2BMm15%2BSPVgWsMVORusqMxJxZiR0roQOez%2Fe1os7zIWxFIlzp95TchPQjnMessYl%2B%2FtO%2FkPRrMdFqt6IJyiH%2FshxjmLyMoJumZ2UspbbzR7j37hcEVf2wBzsvSngYG5gNgRJ3hP0eCwgwTT0S6JTTRRxGdZdH34UIcP5vjCW%2FBRdBBM72fkZmF9%2F4zKpN6zPIeGTuEmKiNp0TP5vEO4WqZEVP2i9SA7SOgQQc534dhFtjEL4sAquBBkfks76oihHuGZIVDL%2F6Ckc19juVyb37%2BypegNZ%2B135D5PSiAauaJuMrHRQWR%2Bd5d5awVp9UUe4R36%2BXFrgXm3aPJEY4FsnFZWlrrAXi51Nivh9NRsuMRBlkfuH01hQO%2BtnTgs3QUlpmetoTHaz5LAIfNpnsOxruv5ekWzBQojESSRrzLEXr92gm5VRPjwTmN5JpQSKdtrpLY3uBhQYgcvNkjCOgabHBjqkAWSaS06L8Z7xphPYwyw3UXzvbQ%2BgY%2ByInwhzJbFruCXII4HsCTQGh8h8G2tRtqKTg4wc9ONv25I3bvoeU1rCKmPnnv4AT54ATw0%2FbMdqCl2Z%2F%2BQJEwdnbSSxKKUgftUYcs4ALg870TvcvZXRvd2NdfWPeoPyVLpKE9HPeR%2FWoGQPtV%2FShhVEYL8pljnrYrrbPvUX%2F0B4SNtWfQsYBPf5n4QMhm72&X-Amz-Signature=9d8795aef9bd6544625bae9a1d51e27c6f60733fa6155f0d7a969c659a028438&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


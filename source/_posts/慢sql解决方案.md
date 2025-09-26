---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVJ7DYS%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQDfg8VLiDvFh5IzCtI61Lv81oiWIEILD%2Bj8Fm%2BBZevhMgIhAKzckQ2KeIYkhuxbCI3ztUKgR90vCzF2odRfS%2BrbfFnNKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBDcOtTvJZp0Sri%2FEq3AMJgiU6dOt0c%2FT6rHb7aB4yqK44aGGzLiM0oHjrRmKkxs3WPDHO4B5chqXrM9wdUtySxaEbxVRWTaNiQNsenlhoRT%2FUmVYT8jXjDTU8YuFiXlRNXFJbFU821ZCy4%2BCRFIxJSgistCq%2BP0EZd0NTVp2OgwnZQMbULK8i8FqOmTCrwtlH8%2Bp4woa0W6yU2TnmQP8FYw%2FgGEYgajrbcgwrI%2FDy%2Blq2JncSO1FOanXrAzQKJ1ZZe5njU3Dr063yEW%2BMYaRTRwix3o0kgyZRDarSqyWdrdU6seA%2BnBjuP%2BeKfYo%2BFwAqxdTwFIvSAWpEW%2BqIR2Mvr1tpMC%2F8aHlxhsBeGX2mut1lET4CvZvqQOlLDi67iYlajleLWqUso9wN7rnixZuAIWdK24MuP2YN2JogdwHmnDMsV5jEePRiwkapSuSf%2BcizHs80UZjGN6DHdpvywD8m90%2B0u5a4Qp8npVtcirJ1IHhdgvo1kom%2FviKDPuAQg7HRH%2BxqkFJQh55MW7txvhB3eUKv0Br7gt069D0unm3sXY5v0ZED6XBOIr%2BFLdXQOlGVZsvKsrVW6EuRRLcR8%2FhhnPg4lxN4Au8xnpKid%2FYHHFBE2TP59BfQwOQLImhxKpZ9ZurtuEE%2BDX1H%2BDCy2tvGBjqkAbhXefOUCmUzQXNFKeB9I4AsZpFyh4t9EAhl0h9U5jT69%2F6IW437IIho3bSK%2BZycpFwIPhyozp5RIznI%2BbI7nLylzggogamhBpVT4f%2BNfhT8XgzGWFuyfBbAVz0R1pFaOy35FXC4hCSvuECb58DvidOdpDHZz4yYozwyHE40xZKXi5eGMFxsXABmBPq%2BaMXtKeQcWQhHIoP5toBbQzVyRpBcKSDk&X-Amz-Signature=5e88b00bf1770f6eb81010a1130f0374ff377fa3b7dd9fd77985514fa227888c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXX4BKTM%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T090038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWFcPQ5LOVfTuUlKrfBeBi2oisKVxlrLIVHGavxEsMKAiB34AE0MooN27mK3JHs1KgpkPeOvvRnakHPd5yPc79BOSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfmBrK4voykZl3%2FgLKtwDB%2BXJEwgHSLiv1yv1x09G%2B6ZoILwz6SUs9uxfzPB2Ulc8NmlX6K4GUTAfArXk7ObV2PsEw4gyHWjcGIvTsX43ZD50wIm4dE4duWMtx4V%2FzhrLUR1KxEGgE5oM9X2CrNIHqKBLKI0EoBeas1po5OgZS8zWhZ%2B5ght%2B%2Fi1HKTOvhKg0OTsxO%2BWacv885Lw%2B%2B6cUU3YPo%2FLp717%2B2JRQ41czvVrvWSWQlQRKNsNBUEBWooNOl22UdOVW3RB5jnBGaTUNW0LAUf6i2IwoSz0lMQRXWS%2BSRK4ZwTJznkpd8iO0l2eYdHJdtCWWAQRYdm0AxlzFjrhBlHhDVYHEvCjwj8D%2FhOqYkX5YxUd4qyF%2FbayS%2BYtqmAK99ptoepqifOcObbc1x%2FRCvzGOGMT%2F2unKiFd%2F639rXXTWH0aDosuPPEOxZHmaqH%2Bjscs3c8OJaMAMHexihIiipPK0O9JFDOsuCLLFzBSuLtOUYaaM%2BQX6Hey%2FBIym5jxg6aSw8wW6eyARP%2BgEP7Twk6GvE2EItTmq1h7tN82IUvVIozGhybez9HgrwfflBqRewPmHLyx7hdTDkTB4U4UV7P0pd8lxELXYCZGVmZt14WMly5T5q91ZPcrjCgkZ1DfaX2hn7LAkNagw58HbyAY6pgFfvMMjkudi5qS6mOwx%2F93XSmLj%2B%2FuQxReA6de51o%2B9Mo0K3YvGU7EEn4u1f9fcco2mJGPEK04xAqJGy6DeP12lywAVW7yzNW440ljBKBrf%2F9tSHpsPwrnAqvVCv8ZaPsEag7%2FjG0fVE%2FlT4iR3j5w9ZugRqerGeXp9TmmftxN0TFE0yZZqNJl9MRBLJ7bHwrswrY8y4i3EujvvJwwO89Ptf3D7nOiV&X-Amz-Signature=c9d0acf086e4ebe422b730920d8fbe30b28608b9ac27b651e0b7e1c11ef2a9da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


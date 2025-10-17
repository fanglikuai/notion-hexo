---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AHOA4ST%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGgZB4sJMz4a4W9rZPYIpzQONwTaOyLhov2ORC%2B%2FtL9HAiBUGY8Rp6ml1MOP0mFWQ%2FjnDVWG3V4ASU2QVdC6QWTFpyqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2F5zr%2BqGSW7nbCzb8KtwD01jvyvFAiag7O9jNt3W%2FWeJ5x6QpEOU2IZ4om8o0uvysuYSswJfQhEyVs%2FHcCbkIDCbNWaP8R7Cy1DaWILFIvx2DCSX6%2B2ReJqsistPDpeYIndUyDthXglsw1n71S5RGAtZIZG8KXN3x5qh0oyjDeebH1LxrxfvHHVYkPfONLHrzprxft7YVhktGrebOv2wzJq3giBC2Br9wYn8RyL7sbKLHoPwMubga8TLtwMNnE0jqi0cUMGQuHW5N1yy3t67zeBbroCH%2BuiwUzxS9raTNS2uNgxE5%2BLUGI6uOHz7yvJipAyM6bL2sZ1KzILb7Ehqj9o8OsdUz5NnkyNPWlHlLHixFBJWhPX2%2Fq8Y5wGQdW9ruBvGhXmhxW4BuSpgoIqTFe4AzMM4QvD%2B01oi0h0bY3nM6nEDe3zqXBSGuwI1drqkRif2Dh2luQ6UwtM%2F%2FMKugl9geOXhIOJGztHlpcZCMRd998%2F1NlTqhn3D7fkLZq63%2BhLgy4a69BdPpQOg0JbySia2M79rHJf1KpyfBVYusYoJVRjUauy%2BzCuJCRmhKw1b2eC8b74MO5BJi6JD2bsv4lm5ToAtqt6ZDOHPFhArsu1LzKAkipVYog4lYWS2fL5Jqyjher7BRQu9X4GswnObHxwY6pgGCR1BZiP0FqGxEE5Ezim6Wq7sAGSMFMXR1XhDIbNj0FJa9iBtC4ast%2F6S4E0UF2nMNy4S3sk3s1fGr5drZfVy9yG93MS8PmLQ77MQyzcMDLDMsxmdw2Cw0Mfrk5RgkkiXDDxQqCMRVnDl2bbjBo7gv7STwr%2BTRggK9pNKLATar56BeG2e%2BDxP%2BmfMkPkTaZ5K1cAXaWCqjIw6ZgqvwscgBMFtW7Tsq&X-Amz-Signature=6a4fa6d251b7eef364f7667416101b96a4a481facc1d899d43382927da917dea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


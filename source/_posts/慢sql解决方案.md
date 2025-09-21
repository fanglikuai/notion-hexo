---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXUEZOS%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSuBuAq%2B1y4uoksjUsXT0UHPI00XLExRB13k1Hfa6R%2FQIgZpmmH4hIUIF4SVuGbrV4jEpBeEWmUnlIdKpz1TzC%2FTwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGvbTTYYcwOQGtjodircA4Ps3CgFNJTiTc5O4mDhLD7TwKzJcfdrYf3x6gMKwsLci7dEp9riLWSFd78jAu6AkR8ZsaV9ok2XQt7W5Mo4A5%2Fh3PTlfe9oEpN0eHr7ySl0r4OjVy%2F30g1My9%2Fw7hKSHw1eg3NDx8sU5B21Ih7ihSxq4YjXvihR%2FiIjDSDKb5%2B7WOhNdv0LhZ3INpsNWppkXTXC0WQNiMYcO18lVxrbviUcSaJPXkOLzcZuJNDYOmyFLXgVl2yDvLUU1FNQ4A9HulqbVFhS7NhjrpUUgzW6PpD%2Bpd7%2Ff5uiccf%2F0qJkiKLP6aqd3aB%2BCTa2N03tVF0i6kd0%2BRMFjRYnOQ%2Bd1bJwd2R9n2DF6oPxSIJKVVq7RwcqXcLJyhFfkuTpDvlV%2Fw7iFWMVeTttI8E30bGRqTk%2B%2FMqF2Z5Ahw5lU35luS4Mdw3lwTkKFqrrswF5qngyuF8vS5Wab0mB9YwVCEpn5Pi0oFm1JlJsNRN%2F7%2BFTNUVgPBhjUkpKbUVUryylg4%2FOl%2FpuoZ9zUOiQgt6wQ%2BRVs5Ld0nfLt5S6z4Fm5xS4%2FZ47HAUwbzJP2wxTznjjimJipijYKoRdYouguEPYIZTuUgndckRwJguDpxeeKFrgRFw08BjsP%2BwYGDeEBQ8zhYrKMI3gwcYGOqUBgimtMW1v7eos0SfP8%2BAA6mzW4qvihKWghtEZd4nzkj3WKX%2BW%2B8kXvBJOIixyChH%2BgUoBDObhkzku5G6cn%2BrbLkJByRUUckjwcYiAsPkloGn8ZhFu1HBmojx5qzLZCqwyji%2FFLGAU4tKI5ZJDUf9342rywFg8BYC0mnUnkg8ZJZppWSLVMBQwHFD9OYlqcVP1fwh4ShGqXfQm3r1d5o5dckqqx8wy&X-Amz-Signature=6d07459ba8dc3cf71ace8977b85fbe54a757a3dd0972d46b3ab86df61a8d4d1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWZF4JZG%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF7HaATBXnAhNYhWMiDQM%2BFB%2FFm6vfyr0AlrfUeQLhheAiBIThsv3EVkFRMniwa7oYmkXOe7BNwl9UPl%2BKrARTModSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMn%2BUrRB00d2lLFRkXKtwDoURwnKaUZ0JTj4SG%2B4eHPRhO9j6qBTJvBlQAKRlK3P%2FR4BLDeZDxX9nRta6DTJI1Ab5PudVBBnbyky5P9sb5dkdsjtgenBSpp9OXJ8%2FVAKscfaERqzQS6L1ViFXNtbDi53SBEsPHcpKsRLdCdLv3Euxtia5K3x7xckNG3KvDaGJB%2F1x970N9gaFYH7bWtbH4HXYE1gGEE88yC%2FYr0sLjDijtFy%2BmURxgbjIt%2BaaOMGkO2lE97Xy0%2BsnyWV3GpAVmFN5sZ2ZhPR79z7FQYlJAju3BxDbLCRpUh0xbHSzrpzxZ9Lfv7SWIdoiHNDN34NRfrUUwYVYN1sOo8%2F75mgV5jqJUfOollWm%2BxaahiPfs4n5d5tnFAU7Q4U8Yfiu5eZD%2FsnF7aXxxWvWrZrr1ZSojwygsPhV1m5PZpZswXKMQlJVUJ3nNcukcPSwsHDkc6N5dBwFTTiuwtyoki02EaoifhoCD09KQ%2F6Ef%2Bc%2FhffZ51F6TN%2F8gyUVayJgfAV%2FPG9XM5%2B%2FZPs7ff0gw1gTINJRgz1gKdfZs8JouAOXGUEylVPgrlqpq86py%2FU73W3ZSArWtEnrimR9Kj2eShmJ80FOZWQoLefLElUoeIWR6DO1wvfoX93wR7pFB%2Fa8YlJkwtLrayAY6pgHAKJ36P7k%2BFKK8eXiL5zUodgLwwEOJsmgAPvlUAOcbIGfTMJ1ChHDHRPY5VGZ6P7LRO0MJvOcY%2F%2BC7vh3K3zk1L4odcmGszLOHD4TJurUr%2BgK80PfaP0GP2R6HFc71dbulFvYN2JyBi2kdN%2BLXqshvPXhfO94%2F7%2FbCwWyteEcpk1IVZ9rWWFmhvGvvbaL8M5kodRzkBuUA40x%2BL999u%2FYkLzzp%2B4nk&X-Amz-Signature=38a01c0b549de3cf6229f8da200768a192a1c9f37469582b5ce302e77e54b81c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


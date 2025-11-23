---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDB6DANY%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCF9rY8SENEtlX0NRGrKS4lJm9kNhOBl7vODMXcBDUcBwIgWViHW7nLk4SL%2FE9lZjUXCBHcWHH2xxyyL5eEtHrL9hsq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJy1ZmuZVj3Weg9dQircA8ajgoeNjO3cLTww2b%2FBKtgcgmK2z8xLrl6J73chyXhlcRihhB3BJvXmR%2F%2FGCTuRUoo8JOkYAMUzfq%2F2mMW501hJkyEAQtwSuCPi3OBE%2BIIYNP2sFaNfQ%2BdesLjvGGGMIH1PVuCz1LD63qG7hJzEwpEETHq2C%2BefcjRQUqOHkJxkG%2FA6mkmk0lRUZHflt0AaMMma%2BenffoLYXt0Wd%2FoTJy%2FB6xM%2FkNKur0vCjE40I5Hsu9UWsHmbLlYUtuuw0G1MY6fhrWqjI5JtuuOPf41VUERGHNSLrfYT9yQsJly7vSsASNC68HVWpTumAf3Ns%2BCxTc3jUXu8pYZMMehfNsf5anyoToV%2BS8GR%2FLLBW0QRQiURDAWqgPYe4CmwZdd%2FsvzYLLbR6bu%2BAxXbhN%2B2TOWDABqNdY0QRbCe%2FcjMvEPAI9Z1P2kUwpTMX1ysslruiPwYNYbOJHM4vHYFa%2BAZ3ruLoxJhTj1L34t%2BW2f5KLadi2jKC7cWT35MkaxNocGGqFOHNRLaF6z%2BK%2Fi0yYtqoHrK10L%2Fm4TR5yx1jozK1CI9NmCWgj%2FSI30peW0vjU%2FxgNuWOYRmOy2vHclx4QgZPLwqbmmcb%2BBdXw%2BYlfmlD8ZP%2FrcgQiYedgZdKK%2BqlQAxMN%2BWi8kGOqUBhe%2BcvfTzReIxPdNxXTGM%2FikHiPNlPHYpWnn5EVUcVm2KNHTYniHzUmolrYPR%2BmhsJEA0dHLab544Nxw1KeE4LNFTNUTNm1JX%2Bag1J5AbCnN5rBGLWwbcCqOG5RQ4XI3rcpt%2BESLbweAj9S8OcS0LZG%2BsZ3lTIVIrK%2FcSZA6YfYxSn956YdBYNx1C3rU%2BlP9WbGiqwKPkgEk2pN7ye07KJCE2UDAZ&X-Amz-Signature=dc42232f1ff0ea30bde5c2f3b7b00f7bf891196b3ce94b5a4ab2f1ca50193eef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


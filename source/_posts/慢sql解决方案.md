---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4Z3PKOX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQDHCvXRL5Wayyue5cspdtu%2FlKeaF2hjKkFxSwJKzIYkhwIhAOx%2BKKzhEqOGwqhah2Vdp8mzRAB16cQn%2BXkO1SPJ%2FEhXKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgygL9mc6UjCezj9WZIq3AMFhzhF2McEBO20xJgwRqRgytMM0DrdJzvbaO2TJdxtlbXngIlajafonunhUbQ71pdu6zLRJY6vDItDHAl24aAdapqZ0mUZt1DHgiI6ZuE5N4ziOLhSZcGQAxjIwkfN9%2FYzTc2orqAemHR%2Fe7EGxOFySQrZ8lKjPHoWj13MOQYdPah%2Br0fUgMLdou%2FHotSwejCnYryLxT%2FKZVGRIUBcoq5jrLRC99F33vfCEw%2F4WEAJhXicMMWW4hjzRkFZUSyrdNJluqLf3630%2Bg3Mf8cm7kRqrzcxBEUgdCLPwack1lBKUwkEjAPQUdfVUeyl0yLo%2Fxs7Fn0zo1jISupwMdS4MB9l5aHJenMW5wNV%2BLPjcg9Ag%2FljkGekVBpm53bYeaR8F%2FigKP2fviEoZqQ89yV3cXuUbHTZ%2Bju0%2BELg4AQ88o%2BD7PokbKSMdpkuDk8aJLLhrOf4sukmQRM8sN05fsPyYwScH%2FMo3mRPJLZrG%2B8UHnGedYJVkg%2Bqv3D6nL2KBSaoxmkJqhchrJW5%2B8PEJRSJEqolBjkLu56cht3g%2F6ksT0rdeaIMNqbF%2BQNavFNFKl5jqrZrEDwLG07rHcsKg2Qow0gJDmy0J%2BsJAJjuxbeF9HKWD41OTmfOrkkOjMMEBjD1yKPHBjqkASL4xhHFyYrt2PDJ0m06x2ej79OFpundt4vH1DK1WA6Ddf1hYrhUQpAjOOt6D1t7AXrNgdJRgMxycYbdS%2FlKJr6CN4jjW%2BWYM7SJ%2F9Z1r9r%2BIQ3LKiR6i05HaC2Kku5%2B0cycYNJ%2B4dg5YBGXx3raOsVGm8B6faypWWwFuPN1w5NzxUISBn%2FWW9%2BgGjJU8rgm9SuMsyjns97%2BbqU4KbLZTbdwuFLY&X-Amz-Signature=fb29e12774ada3065c51f74cc23c59b400c8b54a8aea770b8bba068480eb85e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UP4ROHYB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDO2PrWiq8cMe7rE5YiUOWU%2FyGejWDhKIuVG3F4DZo6EgIhAKpx6o1Ap4748%2BUkC2hWx1CdQMYOVVP7t2hFdMvijznsKogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwQuA2IqacGAYIx4iQq3AN2xKoOuVH3eIFfBd1l0vre%2FKxa2vPrQTlbPR1ItXEyAZsPFDu11BZBpqKgvIlcHSPZm%2BdSwKOb4UI4I56eCE1Z%2BcuWB460LSzEa8LlNPx6hDF7zF08ZgpCf7QuSUMK8gni%2Fr5BObXKLudw95qzFKQEXQxaN4N9%2Fkf6rDjjjPNmJiJh2uncDcZXJ49bm%2Fq8owm4qz4LT0THACJbs2%2F1JHjDgDAWKUQJd3sfAgCHtTZSH7fFZKWHCfOAmqnRzgOfzSVjOcPkuDtr2Q%2BqbGZzhb8rlj1myQ%2B%2FVJP2POrEmeOuPZd7WSaBzOT1nQWqIfi2MHLgebznyJFWpmAbyFSE99YdMD4adnrnmPIh8sGgZF7dAkny3tKFqycyzwHp1bRvqe7Sp1Ap6I6Hun%2BanMtpb40qIHzFyXiVW9woih9ruT56aMJZbjnhEmacNrPJ28zeRM2aR0yq69YkA7rWN1XsIWWctZu3GJROQTeeYotoSkldMPJ2uE9Ht6VpxjyR0YOuIVASzE0GVudcuOG1LjDAc%2B1feR9MfZ2TmCnPvDhBb0oiH1sbFgIkS0ejQG2aXUMsRjHeMzU2DwS%2Bc0806qSfrQh381Hl6lVPG9ccbWtC38hl9j0bHLre4%2F33niAZDjCSmr7IBjqkAWG1I9LXciKqlFHpt1Q9%2FXapRkXrDGcjnNhx%2F%2B3MegMjZqKBaEcaGQt5yrwL0IZ8h%2FB7z%2BEJEzRm6uOoQDDlzq4t%2FVuVKEWCqwg9LliRwI4CMRCS73ZVa43ob2Xelwk9nPhicy%2Fyl%2F5yNndaEDjhU7xrtMAW6smkS%2BMXA%2F46Vsie8aWm1lUijNtr3M%2BGsW%2FknKcRGNLSMoI716USkX4hyipLBcOt&X-Amz-Signature=74478a27173a076d9635ab6e9fb85d9b97255d5af484de19fe9a81145ec863a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYQO4FBC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIGE%2FrWg5reaNJ3zEZVbsJf%2BcKUqsYfJpiBW3rj5qZgmsAiEAsUOOTtly78DoM3a1d9UbV1o9Q4ZtoAjEQP1tQNmirqIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbeU991zjo2h2jFySrcAyM5cOBTwkHNZ%2BZ%2F9LOfPYxegOVvZGIJYF7mH9A8QYjeTRUSl5P%2F4v0sCb715oO9jn9SH3yyD4FIJVa4ir78RXzBSoeiuEUD01xk623haovRx0Fe48KsNvJR0k1vhSkxdb65yQBs17mecvcnm4C74lIEJedg3X0Mq5Ox6PZhnlKCxyx%2BmBpjc76nhR1Hyy%2FWznb3k3%2BRTLwAFFi%2FN99wn%2FOgQ9D7ugfOdfH8umQ7t1K2OQXbXX2x5eMZQsyQv7QOjq1pNSU2IyyXo6H03srkDh6uzW6ciqAvaT0tQth6dSna4tT1mjPvPxhpHYvM%2BvqaC59koyz4GrQATIeTWJMHfVJU5mkxu4TiE9EnCQh4Qci7rkJnKxWcZMLTpPPSgqXtbIZGISVUfxdJGUirGTPR5iEEHcJWTQ5IWbI%2BAdEWQLB4uWd1ggbviTmDI%2FATc%2FIXbSGGq50D%2BTYQahMf%2FBRjluzJOLf7YcHFvFSd2ZcqasmDVCcvWoPYelVWFiwhQGuC4Y6pf016ve%2BLF0HiLFoOAdGG0cUQAxFmLsUjHqVJ%2B9VPrLlp6ZWbZSj9HCZH8W%2B6e4zDKJpNCGV6Sxe5udN7Mork%2FlscE%2BuKknyT9A%2BX0J5Z1wwOIiN19R5t4rU1MNyIzscGOqUBRcz%2FJvFXJ4iyIQX0T%2F%2Bc%2BTuI%2BQjEKO5DddotKL%2BxPwxfrSSwip%2BSUNm75Z55XOQ1XA5rrejcFVPp6SIUE%2BFr7dMl7drGNJoUOR5AdUqjg3E2TVDc4VGV8kzR14ssMpRC0n0zOBPLWuxBm5pFeprIIaut1IUJw6LkubIPXB%2FiZXgo%2B3C7em7psbfVJhtCvP1OPsq1DDXBStEKXH8wjzM9p%2F9TiAma&X-Amz-Signature=79b929e5bdd7810fc1ff262f9810f564efb417b529649b9bf1520ddbe344ae0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


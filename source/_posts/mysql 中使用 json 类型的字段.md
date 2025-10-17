---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YYB2JFZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDUvNInfEHOZamrFD%2FN%2BjlqYy9zC5kqYpx0L0%2B1uuuZfAiA482YKX9iC8%2FD6YaXH8DoyL47Dc0N6AILWhjkoRrzKgSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMH5QlWlDSh0eU3GkPKtwDZqvFKZCDVbdMEdvFA1%2FrHDvPbgMKcx4FglWayr%2BrCG5hwxElZqorzkHBH5x2spVkZNuTgk9i8OiFSPXV1QGGHgPQ89cke2HXmBxF%2BEAqvTrVp8EGN9poO%2Bfly%2Bu24YmVnA7mqBe2EGMTos1NEUY1agtWujt3Dz6QMcPNBuEk2VPyxmDpcGmmAvwZCTvcsL1Af2he7tj5XdAjFyV5PV4J88cfRlqRHCtdzGc8TlgjiaPu21a0D7WQnly1r2ntEpe8p14OR6wD5ej39l8u0%2Bmv93fDyCz5xMo8gpwmJUM08AWaaxqtMHqyF8KwIB9kltZ2INZ8Z%2B%2FmtbEyec%2F0Y5nOdr77qZP7tRpqDRvJbGISuFMX3QkDjXN5KgWaUBtdx0mRzbvFDevAQFAXDl%2F9baiC1omUgrJXAMkjXZpdBb9ajOAiFOypDT9fjNt4hnEnkN7QzPvoM078t1fr5I6BJo%2B0ENtOz3ipsYtDwwMtyRz%2BmEbj%2FFFHy6Rku9jla%2BQ9I%2B61sRYel01pt%2FfJ8VAFVbL9O7pcqffTotUGmZXsw9O03Cn23S%2Fm2zGgK7HXbSCJVJDos%2FAURlzE0hGiGd4ABo5mfLOqQwWFevAPXNrzV%2FbqsTPH9K07AO4LF5%2Bx3Ycw7JzGxwY6pgGusz4TJSFpLYzccdiNoazb07%2Bz0D6L%2FD48Y5yls5%2BojSb0mXd5vhYi65P%2BAZIWhDdZLklorsBPuOh2iXEpihluqtmkjR7eq4Ow27XaKpnCiSzYjdiVB6MW1X7sbk3eytYaS%2F80UxIn7QqB8uR15P%2BCVh5yCK7O3%2Fw61SotCfezpHrh52xYFxR3OjU%2B6QspQFraV7BPhEezKRBxYvtKBgIT5MjyUfhB&X-Amz-Signature=19f96d321ac89c9adae7e431b9a9f15f4940e91e3883878a1f6d48238a0995b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


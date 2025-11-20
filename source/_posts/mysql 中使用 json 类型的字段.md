---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTRAEBY%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCc5cXmVBhUrU7gNl2YmZU5AoXOqvAdyCYXnQ%2FLegTKZgIhAN434K7g%2FekByjzCjVdy6jPyKHq9UzgZ%2FLPf01y7I%2FwqKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxrSFpbEWTyTOMRpeMq3ANY8yUQFC00EfvdEnFKOlwl%2BJrhGnrMT2jVQenF5h%2BbOqY3t5PJta%2BvZjcjJ0quHCcbXewWvshh06005KC%2BTwdb18kRDgVHdssdfzBJqkS%2BZ6UhVIbyCy7x2Xttm5NcfqGuqRgk5teMpSJ%2B2GOiSt2KBf4ZdzsILQHcu3tj1Vl24bOYZzuZ86l9N0TraadWP9vrDwqsC7AdQ8yT%2BVlo7lwxqEmIacHSTsrfJMU0v%2BBRZkzGdE5CvkTpnLHz3ZbzmV8AjsqkT1Qn9BukiPwFoefeaRPKX6UeNee43cHGg%2BBkQE%2FRWkoeRC%2BaKoz4RipvRvK8Zz48X63zop%2BUH%2BziODl1YHJhztV6OztTN7H2kl9Tr0N9swZzBy4%2FHc%2FWnCF4OBJhD9Yd6clMtUDl9k%2BGeQ1%2BjlsP1xJLCr%2BuSypQa%2FLe8ZAE9ojAxrNRyODpNm2NHrsx7VYsx2PcMHiUPorXtKc9cMOZuT9sgWgsdLLnasez%2BjvsKMjSskCdQDKR4T5BNzFYvFEQLWE9Gz61sCxcbgN9EvqY94QJy64d7Ev4Y13eQLhEAM6C6r%2BDZwW2z01IMIxoDJhjVF2%2BUTyIIO8QLaeXG%2BHF1DljkdyWb6JrhI7T3bZZS8lUbxweTurS5TCk9%2FnIBjqkAbO0kHbullT0GxOJ6HNkl9owldm8XPHnGG4IGqQWAQSLpx8zm%2FXXKus9h3CLCJjLXLVOG51DcxMeLejzbaAUs7GJm5tky97tMn5aKR8V7ulz6E%2FDOhGtjqkB7QwkkVz58IGGSzlrZKyXJB4VJoARz05jznDE3zcAP1OLDDH9m8RiTOYR6oeTTGPc%2F%2FP4NW3%2F8Pe95%2BUyTw5cS1vOgkbocS8k%2FPgK&X-Amz-Signature=a38e68ea5f175f1237a3d1f3f327b49af55232193a7a18ea00f1a20a40afa3c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


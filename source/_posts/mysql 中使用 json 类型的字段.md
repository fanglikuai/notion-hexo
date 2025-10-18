---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ITRSHCE%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIBEJreZ7WqhhaIteyYyGgmnNbBOi9H%2FPsaCJE%2FBzJUuAAiEAyZaC4%2BhrCS1RHLDEb%2BXy5LFcL03OZp5mIzBSKeC3Jr8qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bgf7sZmB4Q1Fb6wSrcA0vYKgbOAyaouyLDGqgkMOGLC%2FiVE6GN933eSx4D7b09%2FeBodCSWXPkqeelRPAaMiqBODyGW4mafGagDXKlL6DANGbpnC1hK7wj1mVWLCTTHXFQ5a%2F%2FSMfIjLg%2BdBABpEcxc0qrBi0OLHiMMEX9AkupX7aFb2HkolW31EzMWut7ssJ2yL6M5pLC4PzXPo1AkxBMilJ%2BZVpTACh1BQG%2BJwOcJ%2B9SUP2ZeyroSWZRalYDdMYpmxJK0pRWdZRsFdHOWxDbsXKGJNV%2BAj3i68kP5TGDSQ%2BC8JohzFE6jWTY8b9gznEDuN77o9IcMAwziP3MPqmgrVqqntloZY8f78CJV1Jp5LAyx1obyqEDi4KjWJ8y2zSNvjLDpEflvw9Ds2iSMsBAy0PLDSxFco6J52pxLF9%2Bo8GIQAc75f1XTfUSqjSSUAvRIvYbWkUkc3SzfQYiJJhtZErGCFqSI6GLlWy7R9eLsqf0RPcBaiUJRyRQVWcZv30i0w4ElycO9KzOuP9MOZ5WswhmMLQL0flVinT1gBcbc425REsUxuALeYjCSIZ9BOEYss9cJnQe5OMjyosrMXpEr4h1ajJCUqv5Y5KO0cUS8cPRoIWmTu80GuhoNKH%2FzoGfuqjyr42qteVP7MKjlzMcGOqUBFjRdspeHOU73vkV7hdJAhdrlReKbu%2FO0C2Qj7Ab8%2BYJGslavnKXFuYdmgyDAUNxDc8FcQUSGBSQZMXXfk7eMOsDesWqmocI0t7wlUt9hY4HGaCg9X1AMorB5rcKVP%2FNJJslQPDPdKvZyhVQqHkZRGkjjs98FgQezdktc4VFe%2BY3lDKq3j2942silv9QajGYLKSKdw2pz%2BowFv34fmlEgyBdji%2B1s&X-Amz-Signature=c50d88fc8f2f2d53ef1db52bd9320ceef99c3cdf9ba2e3db2d5df1c2217359d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


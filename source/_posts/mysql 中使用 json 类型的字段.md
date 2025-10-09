---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676VUDVUX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJGMEQCIGXU10SPV1F4q9eunTe1cqyRohWLF2TpJ9SYEwngnP58AiB83C2Q4FUOe%2Fs6EIWMoc7VyYIBV7fxneONbkND5G8LniqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsfVSbNR8T6kaykOuKtwD6PQ%2BhX37pNHfjRYp2vX43kRliy7ToPLjTgqe5wWzxpngt5ILm7ONIOBcJxJ63yWa8W84UC8LezJTvYhkBi3UntM7zVeLje4F9XjKTUz8tBV7yNDSGf%2FJooiP53wA2lDmfpikOH8X4j4Po1HOyljWcEyauZXroc4RwD%2BFX1q5NBhAWOQ936SkjZ%2BpzQSYNj3K0iZWHxj8sqkS%2Bbv4gdHM1OymMGPM8cpItgzVOBa7G265cJQYC2JSobfO7N4%2B4CKlmNpfNf3r120GlxstIvhXBZOpLWkCTFoX%2BKZcZ%2B8Ji8h87RzPdlY0683Y1UFeRGd5g%2FWbQcJR9i44td6wV9c4aKdA7Bi9W7o%2FBXyzaTZaU27T3ctSHfil2HmFhPBonQUu1qKbF7Qv%2FUrMM1yH8kGdqAcKwtCm%2B6JVA%2FZmKgtMfCulnUVpC0G%2FrwWjqT%2BOdvDCjV%2F4%2B9mfKhQnMWaYD%2BGauuQiD%2FuvD6ejNkyA%2FQFHR2Dp1TeZ8zHGna2fZQq%2BgxoaFcWV8LA1AosMtMIYfeFp7%2B2uV7dYIdJzAY9cj%2FsIuUIjk5ArcIPg9ww2Jxo6sIGyzHF8flKD1uUiJEm4%2BMZD0L0Z0o%2Fs8HzaLyJu7uT445yAWDm2csHHfjGaVqgw6sGexwY6pgHymwf%2FT5Ijd4w%2F7F1sTHikaxCaU0uYC4Q4lubANe625ZvTqf0WTdQ5gTMepMq3FiV2wHaprp4KwssOXQyza%2FcmLRQcRtsrAP%2Fb6iKYfJfdk9uY1Y%2Bf%2FGNlV%2FAeC40i6X222KxmnQIk6Z4NYVvdQf1F0Zh5YojdIs4OXuY87b5Iw75HblX69c0wwveLMngPQcov0T%2FCxLNAZDYkEZ2cqEEyi7pDdng0&X-Amz-Signature=17620d9f3090ce91289c95cc32a45d073ebe92471853131a56afcdeb7454b71f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


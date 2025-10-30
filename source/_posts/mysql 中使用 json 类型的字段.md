---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5UTWZDI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQDPPvryBc3EKriKTBIJxVX4CzHXnG0eNBzwK2bFT6ZTDgIhALZ49kDWVVubzUl1aRckwKhahnwef5EQppj3ZBrRKxQ3KogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyuUr25SzAj%2FKsq%2BV4q3AMEHaxt6C26VkGQOg%2BmgKcRv%2FFE5LoSHU1MgaH0rqyqdcDjwKZit6nCnLGuH223Jz4XUTFRlYq5c2ER2Qao%2BXNHQhr09FFmzs1EjL19NQ6GAEc68URFUAPLbyvwZriPtkWMfKpF1rDpMKqtMtY3sRFO8%2BzWxTVk7TcT85OA4pEbBkBaCifWs3srWjvc8%2F3CYNJz46tq%2B96gHfF052KboU4t4KvNn%2FCG1zx0jtQO78x5Tgle4n4nDovzkKkVIjrKW3MzyLADfKZhfnuBFKDjK2hUKhHzjkqlrqKdqgEYbkdA3YhGJIDrYbSrFcbcgWS4P73o%2BsCLxuU9H46rxhvdZxfS815Knv33wqb0%2BylwY1FQlAvjWkubB58cu%2F5p6Nv%2BqMLX5nwX%2FWtrioMpbYBc9rOh%2FH6u4axD25suQ7qFUTIZYxPNuRfTId00tUXZ55MQ2nq2pwm11s0aluxf%2B%2Ffcm38tSAUsfbQFCaA7Cc6QNxsGeTsRxOb2j5T0XGAywb8lRoDllIU28%2BYW9Y5QQS4SK9DU5u6Z78f0J8U4vRn1pNhTVJ0WtadZiFjkLRmi47IBo1MD%2F2ZRd366zRXB8D%2B0lmILAaqI%2FBB7qWlL3k%2FeiJ23l2CmF%2FMgvHpMTFANKzDf2Y3IBjqkAdOIZorkZdte2CzK6iTDr75yEFnVKi%2Bcd6g1lWm3Xtrf7bY1KnCypHJj9V6KgDrmYo7UhziVfdA9c0nK%2BXfwVToGTk2uVIPlnVXtINgEkjYcHyIhSZpWBRowqA9cT2XjroApnoPixFngyz7o0bUcB52UU17s2t8sSt%2BXUTBVVI2YIoRZc2eHdD%2FSxNVl2x%2Bq3Zv1PZrBuL6ZbsKAzmMhO3svO9xw&X-Amz-Signature=8725fa4552d81dacf7967b94a397934f9825ad63c4ca4ac6c56bf2f192e2bfea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


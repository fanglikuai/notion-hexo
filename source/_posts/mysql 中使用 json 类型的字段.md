---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XC6RNV5F%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGdVUFRZh%2FusPUUF7QaTHVfOzB%2FDyNtR4%2BwVDaZZ0wo0AiAtEcdpcsXw%2F4D9D%2BFnJDihIvwMASoPHF7wqlK2B4gJdyqIBAi4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyFmrAcTaNEd%2BModkKtwDkr%2B0XK1HAlumwfvycvvxadThFX3yFo2V2UKIl3kvqdAy8z6yxWQVEUajCix7jETPwyECr2XMOQu5TL92JYI8ddsX3IPW185%2BHlbY9B6BegaGMGAfXR69nNM%2FohFNoZLPw7UGW1xlWfxl9K0Mu%2BFKegFuBosrXlNtFw%2By%2BN1JMBAOvzFQXLSrrEpBQGKreE3ngrYhUuEPGqnUozYrhz3GsnGF%2FMTQam8nFOjBD3FgCSvlnmYOMmZEAD9gUX4al12UyVDuidIjBKVxIWDdj3oN%2BcoYHM7zdHjS%2Bb0JE%2FuGxLEPdX%2Fw2Lkx43zhCKai9NURxbDFzxZHSAOWjedU8s0DGmNmR8XW%2BzV41hiURZAKobqVZAtDbnwxKbmiuHoPdzPZEzhPhGfFI%2FXFNGbQ6TUHVrcqdg8JmH8ArByo9Ra8kBgOHdm3W0sUDXHd%2FHw9lXx8KXgwjlnYttSAD9%2BbwnhZ91XO0AYqdpMivdWvQouxNEZz9nh9c6tunZZUtHtAHM%2FkhxPK0yNL0NIGXHXTGkKtzZlmPSz3BHN5bdNxABFvD5VfYdjOXbxew5IIp7RddfeUV9J3quRF1wI0rGtJ58nIlc2Y45bOuaBkFbk2xqEwBZ3qi%2B6gkghh9PeQHJAwpuXMxwY6pgGqnDW2diYvLKFlFj%2BzGloNFHHF3e3vxx9dqfKwO61cSrJer3ag4l5A5p2ahA3V1OlCnGrHNSZiUXqrYj%2FH7x01KaleYZFJf0EBLaOtgJeWa0O7qpyUqIRViJlt3raF2anQ4i7znulK%2Fgtin2AFunpEZTIEEglorhwrVP%2Fb%2BQZYthJfkA%2FcZReQnVb7XBahHXlG4i394Le54IxHkVvWCWcTIJVbdS5H&X-Amz-Signature=a42fcd36e20ee07c453e97fde69666a76f537b8fd2dc55a63a7f6942abcbc06c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


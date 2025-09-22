---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOQVNYE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKVMwOUQn3Zm6SOCfy2RN%2FwzCK8h46HVYi07yascdFewIgcOZPjV8d%2FfWm6oVgnOn4l2iiuz5Wgi3BbPElLCy6%2BgMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDNNn%2FABtxhOeZzxUGyrcA4LJ1ToErrABMQScaIfbGqEUV%2FEMZR%2F87%2B5Gdq3jApjaXleqy84E1NReN5xKKuhg%2Bj8ZJXmYf8B0mjELRz5QOg%2F7fteT5lFwLXTYHedCbHXZdRpefdQP1t%2FgudJZ3MN%2BmVCxlEHIxEfAGx%2B6Q0OaCM3ZDJI5zm%2BrtYxZPGIgqLaOA1rHEULlBtEBH8ByIQaBRBREiVehOibRdg5xn2NsWNDFRqd0qbfuTGiLFIW84%2F8ourO5Q4g%2B9RNf5p%2FFxoYzfCRWLOWAeeJKz56pgSpu5AVmDjk7RBZxwF5b6G0doeooytIrZHcMPt6UNq4O69jIXXot16Om6Ki9kJPu%2B2c1hW6S88sU%2B%2Fc7kuWXHMZWldBJNDJHGh%2F2jUj8vZf5fRvM7b1%2BnuXHWFRA4iIizXGZ4DaZltO%2FUoB8IU0C5pS3Y%2BQFRl5F1JU%2FAfvFw51GTFqGy64BqklChr73JU3AR0LdOaPaR5Ag9xX7qxNFoqPZ3KwRnufP%2FOIM90v%2FujsnPDoeTINFsBEHSignSKGxV%2BMI1Xej2jQ6rAm1Z8Dav%2F7DKjWzNu1BFtNk%2BYzI2ZBMypeZi9ct%2B8mX9h407dRBDbS%2BXHfpeeY7jv4R2gsVgUOhPWWWK0bOJZb2WOXPemghMK%2BNxcYGOqUB5ni%2BxGK8K3Oz64u4tVTkuaQOYR4tcYKkz%2BluXEEbwTYr5QPSsJa9tE6%2FQgzDC470NJOjpNA4mgu8kjBfpNPDCBF1szB2uMwKREaJVONsEk4hYUa5UZnl8UdO3y5lf672lYF8eiDXBs2EwyI3HmFLp9YTeeCAcXGLU2ml884WD7nNRiVGmy2vWapaEpeBbgwe004%2Bnh7aD2YJpt6a3j4y0dknvEoM&X-Amz-Signature=0dde5ad6de945f275b0a6d495cd352b86e316d2ef1ccdbe254a8249ed775b4a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


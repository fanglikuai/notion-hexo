---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYXP6ONX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDGULARR5VkvPzgLwD17RCILATHJJIZn6SYdfBIxykypQIhAL3oswDUetNHK7N%2F6DfTUvqSvHDQt2DeO6Tjn3j10GlZKv8DCAwQABoMNjM3NDIzMTgzODA1IgzzA%2BFpOi%2BN6QcZj2Qq3AN7x5PsRcd0C4wN9e9JRrqgXVODcZjsS%2FVqvWNb9dxKdodRc1740ptBMkxhayE5f2TyBe7PbQZ%2Fn59KoX6U80uPAu9WMygd3fc1ByK36rH%2FWCqHkaqcdNt73VVwEno2Il%2FiArj7u7xqD8w4VdI9ZDEmxmQ9CR4EOuDkmQeRDfrx3yoUV4gwlU9y1lud%2B1SVmEJtQeni6HEpo0M2UsO6BLWEbKEGUsqo2x5m1tKzXZDL5bAdDH01gjvbf0NeHo1%2BB6IMnyPKHjsgAgpbHnPQS4IKc49J0E6yv4LhGcbYRSH04HhyY83IRN2ebFwHGcEZF8HjLch188KRhJnRr2lg5v%2BP%2B1jAVOoLE37qod6vFKngu7OS4NBn%2BecqbwZnSuu%2B5BC6kHiHSWlfNj7g8EO2moXEY0OMUB3SFkNhxF1T7iVVuYgL1XSFgzHaX1luySuYNVDZozowIyS2gf5boG6uQ4qoD5tfS2bSATQnQ42sBRHZZxqUwB3quZrQj4lJqFTznByEf7Kw9Xmannhyfr4UkYZNGtThANlFTXU%2FbHRsMc2aPB4kaZ%2FErAfGkaGHYvAzTbqDVRYtdcGQmOJFnlFcSvoy28L2utxD3a%2BW61AGUq4TFlfKW1aBA%2FUJ4SfY8jCd7sjIBjqkAVbT%2Fr8fcnzRh17JCU5GK7aMoqh%2BM0M1EvDoJpARTqKTPYh6qvwRF%2FubSL2Nb4ZCxshpQYBgm7CcvljusJaUlFTOfVDTpLbwWXOhviLHEqtnslpwivEG3PQuBojBdDNVip8PyIU7XpC83sHHT5dcRUnfi5W8ZY1jLM0qVFcL8CSJedzFn0AnB0GD50ReJmjydWDogBB05S0XOex1bVUNscWcqd%2Ff&X-Amz-Signature=d9e2d4a966ea47df565a19c2f72c4b3ae77b24b76cbf90794a15c3e09f7f4c1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


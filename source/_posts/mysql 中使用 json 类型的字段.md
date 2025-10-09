---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PTRUCWX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T070109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCIM6D8dhI1upCI6%2FZu%2BNikzUbfUUjMalwOESJBhJdt2QIhAKkLoxbYBMzwssgzuLVFGrYapZ6%2FmYtJFrqQYkPH%2BVCUKogECM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRKDcsVS46bozEd%2F0q3AN9DJtQgUobf%2BiM1%2BY1o1P05Hrj5DdghkIHCYGtOknAOciKdC0KneY5S9%2FH1u9LLo8YZJ74JH3xPIQ99g9Jgra%2FFrVVYYht76ncMj7w%2BCS6nSDcbyHjHnFzIjbkQ0%2FdFXPWwxpktqiC6lvUNbRKNjUVnsaYbzqBo3meHDz6J8e9AfALuoFm%2FWp%2Bu32j7yLXsQICz7H3EBMtw7PR2M%2FbxKbURAOcWp4pDVUGUEyv9b3vhsMaJnc60zR6dAs0biwnbMXIBP3VW33a3m16UCWmE4nsQugh0580X94Ay3kKX3d%2FPqi0RBt23WnvlDe%2F76fTgox%2FoZp0QHXfaR4dZYBoaf4N%2F6QYaOohSwJP6p%2By5u%2BTN%2Bpe7CoAqFNTfjvqdvQOQzGDcu8qZeblfUwFC4tt2RR4uEwjAZ4Sb%2B%2F5%2BNnWcJDFtOIb1vmIj5gUj%2BAgqY%2FZ4dECBY%2FzJehk5vEpObcA5drP8X%2B34xHgUjNtwmxP6JkR%2BmIKZ5DoX1XWKUPe6bgJWEZw3fnNRX7GCEFHPCAt9MZAJPcV%2FEgJ3KOCj9Ni7VQmaNfQKwKk0TlARyjomqvGEBUYHc%2FgythWsx66s6kn4YDrPPTRxALOpvQz1wgIRvqvDjzXyypYKlt9eD2EvjC2nJ3HBjqkASiorV73D%2B%2BTfk45lPUToTZwPYMvgWVdihXxfRb5FaY7m0FkHdjPOhALQqC5%2FvOJvGMppnGsgiV42m0wVHRB%2B%2FOvNkVXUSO96xL53ilkpkc9Q0mYLDt8ABbBsKKUx6R8jcIgXMONJNR9KZMRG3mmPeqMNzPCzE1iuo7qIbWZr%2B0ieThvlLw4DRUpDPo5jQ84VxG59%2FOxXfI0XyTjJBvBmRtSThnb&X-Amz-Signature=089517db6211736925055b22959bdb0947b47871acf7264162efbe92f2508494&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


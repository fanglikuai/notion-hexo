---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDJ3MEJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGy7LYCTwggoD0KY7cF0wDd7%2BVgwzCqeVPCL9e8vynEdAiBRsD%2B7WS8P1xzWroY66i0Rqu79qIloH8UCKz8q%2F1I1iCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFhlv6EtaK3%2FDoCKOKtwD%2BxNgO2%2BZxwUm8ai5uvj14jSVT8S8ugelVCbveJj1DjMiX9TNiPzHkBuzai6ILHUxb7Um8JmAJgf6FHCDs2Khd9RW3J%2FFLpP6nndwQHe9CAkeM5OAi7McudXDwPVMQs%2Fz%2B46KSpeHAVU%2BPceX36TRm4Qt9f7mqjH55KeJql99slF2ogrplqEH4XMdJFVQ0u7WXg5y%2FKQMCDAhvpG9GWe2v5cXMgJyqD7SmS6eoje5gRIZNnxmKKTOC%2FkUkOWHx7HXp%2FM6eQebAKFYdP4Ilqg2AyFPGEnaCELUGEHSCvMnQCPcNTj%2Flx3ub1r1GOIArUe6HT%2BjWZ%2Bfw3uvISl5gIE6aJcFmweycPrbNcS10e83%2BVkULkGXzKFUUDTgkQEjIE%2B6RqX4PvgiGAqOJV1WzNkm9OL88iGiwdbvWVU3uAI7bIK6Hc0XbSIkxnCdiTKMljHhXxEEJo5jeWlrIGo5EeTy9exuwld7rEiSoC6UyhafrZY8p5mpK5gR9Vba2DqO4UYPWZyptgkFBgpeq2gYXRYpv7x9nabpyGAJUk42dF8TPNmLtHyWKhlbId%2FL5Imt4uK0EpYPC7geSmbAHzijTzS6e0XNiYZ3eHwV1WnH0M%2Bo0P8G4hRbiGUuDz02X98wwr%2FkxgY6pgFgvsO7B3sbN%2Fn09SgcYSQBZV96H4GMVqTDmGMtmeb6eJQ4MAlYluTf4nBgI0xSDsBPGM8Kdtk%2BwxKaMPfHP0biHTBAEnQKlqnnH28Irys1kHxRDc9HJAYvn9ZrWQnXnK8C4NwenBE0BQ%2FOexj0PdI6tLB1kwzupQZgivGtsmKVIUE5gLKAE8A%2F3WlQO02uS9oZADHZhophp5DABKQdWTHUySwbyONF&X-Amz-Signature=57b25538eeac6eff261d32e516f2326a96ab673e9a7ea4b698c0228943126e0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


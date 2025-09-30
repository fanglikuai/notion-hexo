---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLSKNI7%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQD2fL%2Fk6WlWlWvqc%2B6b4p377RaafZVkoTJaYOtL0T1eNgIgGBSyPQoJa%2BriCGcz7xRzs1Es0zTPqsiBSgPc5ddzX04qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMqw%2BoQ%2FutcLPOkUyrcA8L3EujGp9QfnNbCmKe7ME4JUh0u4eDg8Z9hAP7wQ%2FOiK99wqj2R8SYsqDCuK%2FGlSwB126Y47HOrdYA7RgvMswPxiWNWjKPiqeXBC9HH6bnv9YwkFrLaFi%2BbQDGTfA6G%2BORa9CUvq5ch%2FbYDyncoY%2FmrkgRHfkLtUs7ntHiTeHw3eG1vopiVAOruzwswpan4o253aLEjWKcI53RUX0hKU%2FLiBQCCmaOGuWTX9LONAT0S16jJQvksTGq0Lm2KzJuwrLDq0gKUojPYklMjUvzt6s3Cr4kzauSgUzhDs5fJv6hUew1FAe20R1dJCzvqLjZgjc0k08Wo0uE1RYLrV94ZdI8Sw82%2Fn1rhM4BEkS1aD7hmm4xpVxYMIBaVb6zpEwhQJxfUpySve%2F9wgJ7%2F6cVcOMv7PLl4H3A1U%2BEGBhJZuFmVe%2BVHl18d%2FFKiSXAHsBWKKw8DM2u60KjIgp4K70RGbkWHqrI9%2FoMnS7xcJEuPZu9o3av%2F3ftgLkwn0XyJDG%2Bk%2BlYzJajS4QdnM6i6oTg0WvA3OKh%2BQTQzhB6lLmw77ZOL5vG5HvQPkpd6%2FA8VDDPYUbt7fLZUGTiUJJfEZSSa9vP3yEQik29abCNxeFdFaNNVcqmLMTsIIOahjcWVMJeX8MYGOqUBBLNu1peD%2FdL5xdHKLUz8rMC3vJ5K3n%2Fp%2Byps7j0Lv16y9X7aUGuKxYix81yzWNHcPhqZ37PksWlIGOTIOhZDEcrUqJ9CLbj0pohSycmM0pX6aFiGbjvol6KB6h%2FL6OQL45oQApLhBDpOktqkbGIEXVi6Zu6EIbEwjXOepxjZ2468aH4FgpOBAFIhvLFLKvFxYGLq4NrB5z1wnVWd%2BvaBMAFKZ41I&X-Amz-Signature=8c34f0360328cabab15a5c2f6a90c39ed0fbb7b194ac0a677c8d4c8c8fd7c26e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


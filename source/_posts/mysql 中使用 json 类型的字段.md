---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEFOFLRB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIEqYRuATGrcxBkAyJxqCqcTGaaWD5wll1Pg5DVGXUas3AiEAwjIJgwKvvIixS1%2FyeyR%2FjOlQXw2ydY37kDkwgmwvtEIqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD%2FDy67ostCRY7NDhSrcA1QmjaDfoTimnhf89dsjyibAWe3EAC1V3QGO%2FawmMuO4PZG4G2rM5dXjq8AFa9qzZEkWYGTxyproWeyhT3k2fCBR9XCqt6VgxQ7EOH2fbtJv%2FVB9oU38lF6RCIQa7DukLnIM%2Biu1m4Mmrra6wJs%2FMkHs2rsnhM4g%2FpOuZmI5eWk7%2BMXFcS5KSdo%2F65Klw6gTZ0Q5SPZKBYQhKyXXEbcvEjM5XhQRY1pV6EJCj8npFcfdsSzHJ%2FRZAlA9xTEqtDWansryEKKOjieKWl1%2BjXIacKwLVQUMQa%2FaFz%2F427qVl9do2AdC273tq6zYRofJeE45Ymo36sVFvXW7nkGNeRbmW84TK5LPNVXEq37gKyvYAsn3tnWlC115KZkqA5PudkOGADA%2FqCFcRO0T3VdLADm1mnp7to8Oegz2uvM4Nzky5KOKr1B7Fuzni7yl5X9%2Fq5aASn42HetHZqPwdLI70KhGE5Fn2H%2FIb3McbwKLRrBTWphiruXcenVmqj95ikeCUHW4bcvXk886Ur7cqfkUM3Um58Xs69ei%2Fre6lmbNHPHm20RX%2BeKLgJ3qNpNtt3V6wuV1TcIUAXNsEkPJ3%2FboCr9aDx%2F98%2BAq9pPyxpkBnqMAJHd0kpZWqYRVjBk47O7QMOWCp8YGOqUB2VCk5wn04HzyfEq5WDU0E4TnvbYiY8%2Bn2JW4zO3NzSWLYkq57saDI1WidZJq%2FSQnKSPCK5RKdQBt5p1bEWbFJHkrh5Lkdy2MVYvNBXSozbmcanERK%2BGO1nj4RR1fc0eIO6TMYY7tedD6BtJHNLHnmzhNWDYWYVuPa%2BVX4y%2FLyFFsCvl6RRDaRF0NDemOpL5bN3XqnFtCx0ek0yfblfQL81R71kdK&X-Amz-Signature=89ba22de510eeb98c6d3d41160de4e746eb6d3a35c20cdc9af6638660d45a06c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


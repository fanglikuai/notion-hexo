---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEIOPD%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEcCFbTr9ekUX2i9WY5M9aGevPNTB5H0Tp3KaK0W%2BqkVAiBqg03V2u39iO5fkjo%2FQ1s7ivh4WVf5BGHXxAwFtrM4VCr%2FAwhPEAAaDDYzNzQyMzE4MzgwNSIMyDIvo5E0ygU0to2ZKtwDDrMAf5sWdD5EWgCeZLv5coD5UQ27FZtISgxoscikwFP%2FegjQitTdbxj0xyxezhyI%2B6RFvRxriyFiHCmgrCO33ZUjT%2F0QCnY4b2ENPCDrdQrTU4Unf9jQXzBuCIPxbZsC8HNjMN%2BNFIpYZtU3ZH0MzbaA1%2BMZYspj6IpudQlCpxddCG20keLgcjM6nkIjPAMyX8RuNhri7zGT2AL%2FZ0Bx2EpaLhJtNYfRr%2FIolI%2FG84DFkKBQAFM5q5MAr4%2BXWbUThm4dKNLNBR%2FAAV4PEBOC3ebBpKabb5xAY%2FdDRzK1ecNX3W8eMDw2%2FVBK5T8snBruOUqJzKcmDoRTPBMd7DT%2BIq86KmFqqwquIZjKZJloGlkgJXuXi5caZCJgToPZDLgb%2B%2BcEpO9BIoKVv7Ojywhy9hUE4l%2F5o1ArwFKfvp7jHcgWwWjszvsSZJIMZPMAJRN5VevDgkp9V5gWOzTLFitcMCeZblXeYjcitCjcZ0Yiec5BpVnGakU3hwmINjO4v7NFxoW0unZNri%2BRrLcHxm%2FSbYgF2x1eBsg%2BAZghKNQxNyUmcsW8j7ga8UnJJMrw7Lyw4epsRGQ51liOTsx7ZsXOHKqIa3pZe0gEtwB26xQnZmY6%2B6gxn5G18vZDDjMw6o2BxwY6pgFtOFM80Vqn8J1px9MeyvVWXdRFDC9uMIoZeV2idJnv71UU0z51UaqCqEbWoDJI9LafVPxE3AZXU6zFvFlaRaTt%2BnuQqHkJmkjmktqzUeJyvvQFJiF9vRvK94%2BAHMXQDK%2FEmOxWvcADSJqEqHahsWBaHZFPiwY1JXrFrKuBLqHm70%2BTz2XL6rMigsZVZXRdOukszzNF2gMkHE1IPKt9C7J6Oh%2BZIop1&X-Amz-Signature=f2fbee690d94eb74753185dbb375804a59101cde6b30eee16040fc22aeb30b86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


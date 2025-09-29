---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJCNDLKI%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T140126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIH87l4k9CBKtzNVYIZrkg%2FYjJO7NK4ymVJBIPn41SNOzAiANH4yPHCygXpOlzk%2B%2FIEntvQBTqcT%2F2cLR7%2F9grIIkVCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOW3qRN%2BCzz6h3MHUKtwDmi9JTO%2FhBrz%2FLqQ7NIakWLVSVBBXBQl35WPya23hXIzTSYmwjFEzXSZ81luOQIDXdywORUQhfXPoqY3XLURYqLaBqyKe6FpCqfNyqqQGmSUXatvzkFSnycjh4pZZBnvfQpzUaCWkmHNFm8FCNYk4Fo8W0duPi55ezxkd5C5rKJJ7I4bxxycv7MGpiGkNcVj2Wxt%2BRx%2FGXpwQC9CYb0npGiZYdGq8zA8nKuFMsNHO1XgnDDpnsX9HWNO0rgSb2snsxNwAtaom4EC10VJ4Tw%2FMdYqMr3Vejk4YDxEbB3o3H9A7iu9TYxWSPVqdpf2J8QBw13hKYf5J0d3Tuyjhv7PHRqbr6hkhIwHK4h9J2Nr3KM%2BPBDjpw1YMfNF3A%2FvFGsbFP7c0IsM5kXmRzQEoft3klq6fhDdJTks%2FDvuvrIekR6FqT6kP7DImv632PeBVtNTWwzK9tuiOOfuT4WcInTsgeWg4BSafKmlyh1ekpDnSGFcMi5DbS8uNTXUD6X2RM56SYeZWgkW92iN26kn99c8FfbZ1PmnlIa3uX0%2FYww%2BKMyEg%2F0A0KzJYDigO8wy1jzQ5pn7%2FvoA62zkPeVugfq4h%2Bdv18IEyUfHQUfCgPtR%2FySr6DZdqeRyVAhW4hrkwyYXqxgY6pgFJ7XAbqKFag2Dhp25ht%2Ba98bDWfYPjKRvL%2BhMeNlnU6mJhc5Eznqk5vq0OU63b%2FdwtQzBWNta6nopL3Ht%2BADSfYalPiLNxWUxXRr%2F90I9fXK1maJjGRBNnYMGeQSdfKtJZ2DMyfpkiS%2BX1zQxNPTCd5PhTqpJESgRBS55YmzkCwNEeKfkQMxSn4nj3FLxm%2FlUoMpcBi0pkOG8ge%2FertIoZ31QN%2FW5j&X-Amz-Signature=7927f5a8ba03f0a2e73f0d5138083ad60a2aa96ab83cc93562ec152e23430ec1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


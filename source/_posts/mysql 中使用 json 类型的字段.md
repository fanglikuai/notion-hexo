---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KPL6U2T%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T160047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCr%2B1tqi62UTkF45fQj4cKmADta%2B5eUNxmoOzBjIVjr6gIhAId8r3dKNao5feqD24eN9JMBcW9LCNlLx7mKCLZurOTVKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9R4hi%2FVy1ykqHVE4q3AM2JPHOwYfRpgXhvEx%2Fgj2JDMVe7HLApTn4fLgn3%2F2xdqML1kYPYK6qfZ7ONxjCd%2Bv%2F2f%2FPWeM35D4uY8yqhghyCrT41INzyNZNjumzG9la8kah%2BTm7abruIDvdjOxmwt%2BRuWvoEtC7C00lqfYb9FpEj7%2BzKA4YeoLMdt%2BlBPeZ4%2BXCcXi4Y4ysH7ZrtCPus%2BYYg1TTejtFSwn6ywP9pr7hbfxrI741y7Avy%2BKkOyBPEjQylY%2BkI%2FzW6%2FUoeW3yYs%2Fu7dUEcmqk429HZkE4mWDHWo2t02nIKyMi%2F7C5XJTfPmgzlHzHgOsFDvUuUI6iwLleoUhhZdQvXaVHR6t%2B45Evk3CN%2B1onBwGc1BaZgU7gdy561UaMHMKvJm4JzgYZOewHGmw8FAMJxm1wDraG02eWn%2FmJzq2NQhA3iF60gmQhyB9NcInhTkjX56vVSaXfgL7iLInG31lvzIE%2FX2QB4L6YRTyAsSlvJnZvU6eSqUIdRBeJR7k4Rs60ukXndty3jjHAB18eaVUUaLt8NQhqjV8SwTspNtiCue22KikoRhMtoY6Pm5Y1Hu%2FLZJ5JRUZJzXzCQHnZHeM0DLJbtdXoxz%2Bnuq1WYDl4zncv2Yv%2FkFCAyoiw6BKUCpqQr%2B%2FFwjDUssLIBjqkAZplMxEZ14RV%2FsulJTL8jf%2BCTAzg0mowy99jkpiyjNNesX4byEZJ2LTIzYiH4c1y9EO1xGz2NixrG9BGi4wsxX09PWAKnFWqxjdB080v5DZmT3g5r89Jx%2F2JtBlK0ZzGwrjm%2B%2BJdKgJYdMd5Ob3MIx6WOGVr5Zen1n5OzN2r7yn%2Ft6Amwu8pH8bouwPUGaqvlIPSCMf8HzQYFdAK0cY0Yl5AO27s&X-Amz-Signature=33dfe799ddf3b5a17221b060f0d6166d275b8006aa8b7e815921b0b95f219ff7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


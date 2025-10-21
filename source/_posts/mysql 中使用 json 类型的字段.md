---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4TZBD6O%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T100107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCIEm7sFcGrzcMmYI5yS9gfIbAp8c3ElAA%2BgRWtEy5DhJLAiAidq42cHjUi7PJCDxb57fe5pI%2FW9p9YrToMdgiJXHPHCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMJDy4oybPzFDRgqoMKtwD1Oln34UqBuZeCQ3OPFDKkyJHOrfVKMalhze8ixoJEX5v2tL1M3S1eYW04XNBjHvRkehWIThX2mHFIqi7lX9Adb3KmItoCcuodWS%2FPyOGxIqftA%2FtPC7k6FtI%2FeyyIKRJ2M1Y4Dh4szxR4ODK4b%2FGrpME7iYPG9slvYYz5jPYcC01WV0lA4E6N2O98%2FUdOoWBlG5JyjlRywUWJROpIxL2qyNVc0zUS7QDpGQUpjFaJtwrYecA0M3r%2FkKwaF1u%2Fln5wHogNU2Vb6OwHJVTEwNnMMso8KvBaRg82YsyMit11g9hZ2C25h%2FFObJjsyrHQyEztTOMcXtoUakGTjWZCWSvx7fIN4fy4Oh5pWeGZiNnVI1QmH3zFe6cNRZqs4DCbOYLrNtghvBauKSbli3JDJd1bAucqLgwrZetI6nI3l8WBSBXHaF%2B5P5FpjptnLMRKieBMkpwD8EPytXLL4%2FFWoNy28Py7XCWQsHV2P4FLCletC%2BCFSuoZa2tvKf9UdPIGdamwiDBrRtJjg4Uxh1kRKZbljt9f1Gok%2FPdokFxZ0c5hq5Ifg80lGW7OXsNxQBNQKSx7HllOMxmui3eYjCxWBdcyT%2BHBDbIIdcfLVOe6smP%2F9BZeCGiGr8Oe3fmDCgwgZPdxwY6pgGDh7YQqlOs8oaOMoz9KE4ods8BgwknuK24HMh0TL66HIGSZLaCNh%2BwZ3tQkrEeHxqCnIvDGrgM8wT0sJ%2B50JZ9weWBVh9JfQC5IKe2KxWwWB9YQuSBaQpBpwfZ59Rl0n%2F44or6jzm77MwOTXfB1OCXGwudJqn%2FvrJctqMfpHMbJQ9oPzQaOmbpeIxhVLx1toMdvgBt7w3%2FiCmRJEIm4eqskCV%2BiueY&X-Amz-Signature=310185326bdc698f1f29ef68ccfc60397cd7d211826882b595fc288e8241e866&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


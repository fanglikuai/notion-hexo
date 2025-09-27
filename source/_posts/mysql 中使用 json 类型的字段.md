---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYHSHUMQ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCITCdOkvOlDKnQ1riifNL4sG3B3%2Fgr1ilD4kyPQV50IQIhALoGYNJeXjZHS%2FweaaJRWTjO9cy%2Bn2YyYcAML18oNqKsKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FtGLW1R06ZbnD%2BYcq3ANr5ABLfWNgLtekHVBCgKMmFKwMHA4pD2kEggwdMm6OQbJ%2FgwBez2b6I54jzA11tu3%2BlRuDpVfCxeK2ExZPX%2BMomBKuPjp6tEi1vrdfa1pIcxsx0zKROW30YVck3hI3il7vpbrx%2FElnMCQhDc%2FtXuhK%2BIZavLW3%2FU6q48wB7dARjKbo9GbycJSAK8eX9aF6djWV0Gi31dbWtP7%2F6TszhQNS%2Fi5WfC6In5%2B5h5gOGVNbkOZIl8xzUCZUtd7ArGOElrHq8gofxBHPd%2FmV5gt%2FQkwCMhfDHrli6I9DeLsZgvUOMVzx4W%2FY1McCpqlUIWBuhAM4EQ1lCeM6qixiErp7cbBqYsjea6DwsmeTIgJFdKoSj6FvRhrSuWeWw4S5q0unqyp%2FLvMb0OXZp9UKi2YOEpsyISX2LPl%2FjFyvnhLY5xp%2FqNncKk1Sk66VVnQEIGo2Fe7bePg1fibpW%2BUNqE4AryZwOhi6hzwS87ZUBeB7lWKvZaZz2gfybtAZ%2F%2Bv51RmEUZBg1iFMPYFg6kqjpxLf2w1Ko8vbJ4g3rgZePcCkecZXbxxiqzgiaeYXlAGyiqlEZ5KPsUgdw%2Fr1msEB%2FJH8pUrcJ62hF%2FldcQEA6f9nksrR2uqRYtzefGKYy2XB0jDT4t7GBjqkAZ841ma4rGE0c%2BrGXxUsqgI0wUQ%2FUDrcXtT57JnHM1rSOTVX77tPLmaBQjKAm50VfuKSJH9ACsBnK%2F3nkgYDGJjHGFtYFdnsQ0wIcLwiSLygi1SEvzPivUrtdhKUCsDXC48TogwiAc0%2FF6VWOrAmHDqmGz134xyi7O0zTPO6Lh%2FGil1TPOV%2BDH%2FkgzRFIOMLe7Cti%2Brwxl8%2FHZZGJR5nM9eYWLb4&X-Amz-Signature=6c1c30fc68cc2e6a842d25f0da1a9b9ff041ed18a59ffc99c7ded0fb5495de2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


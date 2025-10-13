---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVIBCONO%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5fRxR6W8WnJ29gVgLqiGBnDz2u3gDg37hzcdJ%2B73S5wIgA8p7YUt9iHWFa9%2F%2ByatbRjvPkFqes%2BJY8XAID5UqE60q%2FwMIThAAGgw2Mzc0MjMxODM4MDUiDEB3RSnDhKA5mAAZTircA8mcSfIdEV8dGhGFcRAQ8p6%2FysZ8W92wpJBTEHJFqnbjtQBgrWbZHhNlnKjWuaLQCATxDcbCRiyd3kwOwfzyFNhl6%2FVIxRcyj7m3XjAh%2FJXdoxbASotJ98bBwptHzzDGxJXl53gXBW0CVFe%2F9enIIwWfXjIwCwX%2BiVbcnsA3dErj7gjhVn2UtISoAJ0J0B9leGYXf0NkbSyV9qAQNa3LvWyQTnBGWJqTBbzZRNbEm5lGkPb7LjfBabl45umNS0xFE5TO%2BrMTxoEnzWngb6Q%2BvZg3vs9PQ2Cz3BUxJ6n6ZzXe7QWMig4eDW1StDC0ZuWLTka%2BXikjJjyp2FzaY2PpUlWoB8047c%2Bu7qf789d40npFtXXnJirE2xrcbaN3ZBbIej9kL6lIRIOK7rVxn1AqIJLjrm838XzFjeX8qvxhJ5MEkJqNQXaiI9y%2F07Ysvbueft%2FpazS27AaractmvKkH19CQKEls5akYIg5P5NsTAyl6VgAXVntgH7PVIJA%2FjJnJXu5pfyWSHh8e8j3FDBPUqx0%2BD5wbuzJ6Z5p84UgUJam8fA%2BfqvM0xCmIZzuTs0hvRh%2FP4tftuWAGKNT7Nxa5uovPbJQoANWkVrwkr9dDaOSZNzV3pYvffpkTDKTQMIPTtccGOqUBvncuzo9xCWahR2qQvUfsZXTCmnvtRXpb92CwrHjM775xr2E%2F4IrW03JOr9AsHFFFZMu9q9ncIF61Ugy4kHg%2BY8T7hf2tdxamds4SzMY%2FxImrILednshb0U9speh2HnZaxnEW03cc%2Bdho5J7AZd%2B0U5padQZTtB6gBk60gn6hlyLNFTX5EyFfWbhEr6gMUKNtYJ9uv68ba%2FLPWY5z3wYLB4otQ5FF&X-Amz-Signature=b8682f3421815e8680c5187a1cf6843ea6ee5f001004d5599fabc6be2c80d10b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


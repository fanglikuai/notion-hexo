---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWLNDOQW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhr%2FihyR9V%2BAWI%2FrsPBPL1Nro0HRQZKx8dpVQK%2B5AqfAiEAmZGa0ZgQu04AvhQVC7vIN4mVR8Lpyf%2BDPHK25YjQw%2FYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN7lTFOV%2FqsfBPyySSrcA6WTHhEtv2GtBLT74AEJbK%2BopF65TwJac53pn5tP%2F1YV2OCJ0BbEaiq%2BIdfq3SDeugHywZ9HMWshQw8OCo1JNsUFIM1C6o%2BL8RdGwl7wwU8HHUsrCrL7B1VJfbKzH9pPhdtKDR5ED3DJgaSRWKHv5WfOBCRbMnhVdmQd901hWo668yBu3QRE1zJmdiPMt3CcZuxy5deaPRnAGAZzyX9fMWokzz%2B%2FigJkETHJUjmHAfHsX0qsQlgCoMMQU1980yWqv4UKRGJyCeFWHdzzwgocztN6O1fAy91%2BHnK7JL%2BtsCxgUmC2yF7KSdZrssxD%2BJcpc6RbUfjCdF8npebCfGGXmmbypeJ%2FYNbkqbfbMWchEFRSu4yXaZwSQi23NVhmBoh9dFNX8yyV2LtEhHnuyCgSei2oCWL%2F%2BsKtoD0CkgP2iTh5Sn3nlMcsPREU1G9o5jxi4%2FxChv6KnSEF8onOiOYQJrJyNk1gk4tTfuEoGu8268cHrrGmw0zMM7T%2BwtIktQd10sueXkqe3rD7aBSgr6qmYiqUmN9YQgSY4a6FZ505jzm50bBYdw%2FtEk9UPXlq2%2BY7XIrS3Gb1w90s6Sl4GOy7P%2BwxRhcRoVXAHOhE3qrfHZ8IZBLblwiWq4ld6ktoML7HnMkGOqUBNSidkjR3YV8weS3%2BtY4Dyk86lFVtlZZFDdEtXhWlzclOg3IWVWuHhclOdRVjJu214SCkavC9rrw5u1zW9zptyXoJdMmPL3jGRfo2aXvBhvUqFjhnMj7YaPGK7eECSb6JNc2RhjianrZuWGyRFBOtD4%2FGWhqAzwS%2FblM%2FePl5Br7v2jA7nqTwOcIOH4X%2BLRWx4HNnCdPC54mZKgctXM4Ke8ze7qa%2F&X-Amz-Signature=e79d81abd1518b57ca2a76a74923c731fb7e79810ff257fce2cb9a3fcbe93469&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


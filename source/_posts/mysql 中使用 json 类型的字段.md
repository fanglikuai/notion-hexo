---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3LVO4WN%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCVIrEjQCVZ3eh7wMiu5sMXmxV%2FfKxOiG%2FQv81dsO15wAIgCsBGEsVoYE0lkBq0Ue3o%2BQJ0rFSqvthQobbW%2BfNbuxcq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDJe3UrzQDreg5e4fmSrcA6yZzOkFzOegWLUtS7YeeoctyD3iV%2FQsrwZUE%2B5sHn2QsESH1%2B%2F11bqkkDxGeqI8z%2FN9cpH9pplJVmDIqnEBL4eloLhrso7y%2FQaTaF4fVui%2FqGrSo%2BwChiXvExw6TAPHYhbx4WvSty%2FGpJ8G8ZJIqh57wh9Sob%2FaLHERjG2axVeiTu1gwBpb7f8P0L9x%2BTmHdQD0d0AkGR3c82HNJrcqD%2F25OIV2XOmCPFWsbUNEknXB817a4S3rtRfG1qSwmyAbeyDXqog2BwCLUJehwbP%2FjvGUg7NQxHaqwgzCxLMrvV%2FTdV5WSNCNBXQQFJz7%2B5UtVzU0l2xwYrsW3qgFNpLvBHmHb8UWOMD%2BKxzky4D2bxvTa2%2BU4pLS5ZLlkAAK3u5c1%2Fz1kvQvC998wRU4%2BUyhIj9YstkSPiMS1kPv0ddODwiHgtUKArYGhO7p%2BsGhMy01T9KtnmWg%2FdyL%2FgTxHort1uHEpGvTi4CfllmwbTuf0LIjS0KacQGUFKqUnXf8KmLAVUpMan5Y9GueD0CeoE7dVfFsdgta6h0HhsN%2B%2FdDIy5QhFAqL206ffaqsEfBNt8uSH%2B6SPn8Q1Q%2FNU%2FmrM1O6I2e03coV7qJxhL%2FLXZ5YHZgRFfsCsZuwRXVPr0JTMPyNzcYGOqUBFlVWpnHLWUcw%2FY2N4LW4MgSyA%2BkO0q%2FxINX3M0xy%2BIcLtT7HxDP94TACREg%2FXpdoS1OgYp4cQd2TM2q26mZ0Lmf6LqfW5gYrRUylL9ckDPqej8zDHqdHfQUPaSr57supLlFMfHkzowmBwNzw3wBbDWRPaSwWSiBweLtACZuLeoiaOpwPpiOhntc4MmCHNfWYsLmIlh2RY7Qbmf%2BgN6dEzhdGWZva&X-Amz-Signature=9436666cf8f7d4b18e309c1df415ed40432c28a226d116b4f3c3fadf96a220ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


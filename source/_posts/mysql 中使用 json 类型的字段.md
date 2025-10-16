---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UTA5CV7%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T210051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFM2o2LFXeg2b1mHl2h%2B8p3vWDNgePMOusTLAstD1UctAiEAnePFRL%2BOMSV3r54lI5KkaCPAllHYRI4BW6j1HIpJF6IqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGy0%2FTt2XK05ZrjCsyrcA1JMKTn8l9eCdqUJ8AADFNblQ3THRRmm3mPKNyKSmUIIxDxzaTGCG5Fr%2FjDyPS3C6CHZ5q%2B98smPaHqoUKjzPqo3sdcjM3CcHF892WlpMb1TL%2FEkexCpX%2FR1bVZ%2B0AuuPXow32F1qbG42FGLd1UaYxnnelZP%2Fvbuv3xxC3C%2By8AojC1jjZw0KiNb%2B%2BznaQB1v%2F3mdvUrCIzhv23CE8f1aYdJFHwIWvLP6OGKT3AOHfRM7J%2FLM1wNLbLOM%2FNQZ6H5VBeMMmyaUjhuCy9mcKfM9IgQ13BfstjYJ6qJOxWPvBQA2zPAPmvOHl2uIb3ZsI0miw%2BMzvkMGV23cFk3s9h9zMxkpiHoBWZZbO7xqJ9vHLNUltzfRqEAmBD3%2FXH%2BU0I%2B83YORX%2B%2FXnOGsiFV1YIufNfUhH6wrQO3CZ0vP%2FKQps90VGAp21zA94YFJDn9eWu2Z%2BdinPALjUou29ZkNRquCOGpIVYvy5PXppX6iiJrnp6jYja4vDur9HO%2FfzlLApCYYyrXnL5JC2fiE4RefCKZPRCG68r%2BtcMxKwtEbBGF36qoRzWypfqePB9TZ3xUCRTGOTEAeThwz7190qSZ7OneFQmruFvphtfI7c9oa6ykeaQWPRALhYV8uR2ahezQMLq4xccGOqUBvIKLb5RrsZJ%2BZGRNCvtrKUPbLo4nRHOITAEIWtNjlbQqkEp%2BsXPCkbhRnCbzEGzMTyYwV9f4%2FdeduXk%2Bg6GDcToP%2BjxNnmgaMHsrIK917oKuxqvjm99puEoNsCRecS%2FupHFFn%2FffTs4Mime3N3K0iT66ZYgLclFx%2Fun9lI5%2FtDdq2Pz64u%2BoEQ7c7oWJtUUkkLDIoaRmxw8%2FXoosDr9sNjSoS1iG&X-Amz-Signature=7ed1ab553fbd3a0f48e370869a927adbe0571b476b0407c99c95096389bc1ed4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


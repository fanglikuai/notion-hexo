---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QRPL7WB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T150157Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQCQA5PC0ZYiEVIl%2FR94GLchD6r5evIIQv1rrMi43CshCgIhAND7hKiX9bA24BM0XAYBGZMRq%2B7A4CAp2F5TfT3klhQ2KogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxs3knrPdfFcrFZUqUq3AN1kIXzMSmyjb46%2Fl9JAj96t2iJJAr2Y8hMMrc4yvDy%2BFjkPoJlqkXOKUOvgNQKWOl1OwX%2ByPR1hV7dlWEL32ceknMVxRppNcx7SzG%2BTVyOEQKi%2BEB9iMONg3KsWrKDmek99IvDyz%2BI%2BLNTmKvhP6ZNheRhYgqfJOx%2FiI055cKeDEKdrD%2BlnqngIZTo%2FgpufvNItLbwBka6D%2FcBRY3cGhivOSRyutYQYAb7GjJ849wXi2V4kos%2FPlCZhiug0ubpz%2B0%2BcfY5%2FkwYyel8zjMdx2y7C2hNHrSOzKD1jRgn8v%2FWjA9odiVxahcMW76MRiemrSoRQMIC6zJl31shfSM4jCDwnUAEj%2BgAKL1eHivNV58uWIbAk4Yvq%2BVd4Jo7U5743ADGl6sZLGRK0IF8sJwikBz4hRTbqfbGawz01pS%2Ffohar5gB4lN1oDKiSaXw2r0C0oMLwaJToUduSkHsNuzjtuGwOxmoN3cjqPCD3RYbQ5lYpD2Tqauy51R9V29z3eFXxA%2BjUrUR61DAm0f0ZmhU22CXy8unZKpqGg%2BCtS1kjO%2B9cYNhmhTqB%2F8NiXDnw0DjJh6C1j3zPVjZhYTed2DgysnEO%2Bq5Zgtdlp6kifFDtDX5SpNjGIMS9pKAFGYG2DCE1IjIBjqkAVf00EHXCyiuukw%2FAT3BrbN%2BMYLa8SPLZ1CcuORe4cHr484x%2FQehBu2HT3OcZbADorAbwzthB6R3fvae4f3ggJW3JUAWuVlLg5QxGo09MGL04B%2BHCZe0Bzis412b5X6ZOu1upulG9SzVz8PAPoRKrspl9o5G7jjmoxUOmf3oI0gvFC5bQAnAY3tBmMlvbLoix47txaOvzZoqJE2SF7My4bBfOnXi&X-Amz-Signature=a21c0981f540796a8c738fb7600f5ff943bc35751d6d48904303311710908002&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J62YWC7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCQ9F2uKfXvOW66LseauUTAAMgeMRyDjqFqLBpCbuhCuwIgQgIYSDho3J%2F5vpifR%2FaNAOselKxV13q%2BdhUveSkonl8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDCn9foejI6qidGwglCrcA39C3blyM7dUroYIHb%2B%2F0D1beU0RJNXWMMiIcj47%2FPSWtJgHZgNv5%2FZM9GS08IfwdVWEJx7mPh5nvPmZFdzHBBxB98RE0T%2BiHG5lTy86tSm8aCNRSefiosqEFO%2BGxaZ8rSRSQq8Ktrenbxk50OB40fbM7XA1vTfJv9p%2BPskdwdE%2BTNsK29UuiU7wCf563erNL2QARQr86siwaR9QsFK%2FjHqGajjRjrCMqni7TOc8ovqhmYNXTEcWHo6gj8Nt0D5eTSKqHuKt4J471WoOEKDljr4YMrov57i5ZvllDCVQ2W8WbStXjp4d6p0WhbianOvSXOrgpcltmL9E13YSHA0689YIwTdsRHG3ZXDOB9LOO7BOP4gmbKcHHlHNq%2BkMJIDP7NxbE5lqdG5YM%2FAbACrFU6J07clVii6orVyjJAgz54FQvJiXUxRKefxZxrYCEf%2FmKMAq9vkerW0YXDJfe6yzreeqrMsa1PAxsukYTi3Os4ot4u74ZsS3jLgmmLsjYkzSNt%2BzQt6al2ndqsnXS6c%2FKb8845gLvSZY9o3dtFc9YlqZCazE8gZ%2FlkDvSbAP4p2J4M2KwM3L9qm1GV%2Fkq0bPSeWiTlMnkSMAwXuWqGqs0S6H6B0eViGeKGGHViEVMNfyocgGOqUB%2BE%2By8sNQ1UloudjlQNxMsn%2B9PvnJXRq4Dh%2BmtTk5AJSrOuWvc4GlMgKBpqGfCgaQS77UJniTD9n%2BNS24BqJ83wMUD2ut08CUkKEFPJLq4XfwERDvkD98zhn6bWYe%2BTgwOpBRpJQiV9er2xfkhnOHZRYqEOSgGNL1ucPyDuqE16%2FfaBB352qivyiXXRusYhL5a0NfpsoqME18YIs%2FcwIOWiuvSrLf&X-Amz-Signature=a01fd5cb053e6c93983ce201cb5e097f94e85c0e200480145a3036a29e19e5eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


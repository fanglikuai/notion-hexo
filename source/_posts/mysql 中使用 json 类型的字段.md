---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RDPZLEC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIE3G1OLwOOKV8TJlXiwGHovSXhl9ueKqGJLI73dlzkJ9AiEAlFBHR4goH8AVVZb%2Fx0au1RZTE7TqBvBU0EE7HSHx7bcqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAOHFmiWp%2B7PKsevSSrcA9mbMu8%2BIUveVXEAy7udUjBdEOnhqHY8OQH6qTvOX%2FoYEbDh8WF87L1P7hrCoMEcl0k88LCqx2Pi75DZy6RWUWBiWD00XGPCADbM3huPFUHDhr50b2IotrqnJqiiU4X0v9MdMTMWKmuWXjTpg%2FUvvP%2BTNJ9ftgVDFmc%2FG64i%2FQ%2B%2ByVhkhhk9qvl%2Fbc2hLIgpOtXU668RaMuEke6PffhvqcO5HCWAJbNAiMU%2BOdo8nwsyOglbpMxVcue2JL4gD5AHTMJ6sJhzKMef%2BI3j6hL13FQrlzL9LgTDTEnDDb%2F%2Fus%2F93hr9sjYjDo7%2FDty1aQkOCSLn9q94CS31Pd0St734sfYiWAuxEAhdUodxt%2BWyPiu%2FK7leuTkIRc1M20WyftI5TCuqgClg3xH6C%2BbreVb1Gg4SMca%2Bj6p6a5E9KGOB2vmNqoB6RupxALtj7PGfRL8QXDqq8Szw6X8NdtzqL2RlHPJBYM6Agx%2FNPKUI%2FXdhd8YChWGg%2BbJvmDgB9Dk4F%2FB8eVwHcbgT%2Fkxdx9STTPUIwU0mSaCF28PTLrR0JNiNa90ys61H5fEk59uyME0ooCBx1sL3Kzux4L22VK%2F2fw7Q0yMFgTUraVSSZUvaKpDEEXx9JRBtXXHt6gk%2FLncJMI%2FJz8cGOqUBm6DZkzciWF84LRpljRMXUR%2Fqa31cNMdibBy739PEOxI2wEvionnQcFoWJH8UnjihoQklkRrV9PJ3sZe6tdOQ5s2Ao3%2FYcoWnbMILhootlOfulE5xC%2FIw5%2FQDKGtDJ%2FfzFfa33C9fUaUl233TLhPruCOlwEUjYzTuUH3ilXbyHaubCbp2kTBk54m%2BM1wsrOjiD%2FyhjUe8HKM7ZgDbiMo14LlmzuU0&X-Amz-Signature=9f1208ebd3ea93d5707a4820a014aea907e8c3fb339b7ccb3ede61195000a4ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


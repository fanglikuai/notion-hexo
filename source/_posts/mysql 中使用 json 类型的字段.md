---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GUDNUOG%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjQiEnXiT9qxqmO3yxK0c%2FSZraOEZrfmsm%2FheGgKxbTwIhAMhsOHU8EQBQnyY8stO9UGo7evf70Bm5zaAcQ6L6kuOaKv8DCHMQABoMNjM3NDIzMTgzODA1IgwcWHBL2E0aqqiSFBUq3ANk9a%2FXE6w0U9ZjwqBPWdqvdnk7X4tV5ACuHEPeHs5nWKaTSRsQBTnUAUHxKJr34nC6w9mnk21iFGnnG4M6MkEsW5SKaEtXqcR2c0FzIg25GMP8K5zYIEmpepLZKhVRjFV0tuUrR3elvC558ul53Py5n7fUrRfrDMYKwgA9TRag4ZqDwqrLXbyilR%2FTm42kklQhrZxJPYjK6H3r8XptpbVNLrSbbXYJH9CNY0d9fLLRnNhtPw5GSKfh8z6zG%2B53ZjqjginuH8Jbr4N9nLVXTWH3XhSuWrw9PMs%2BCHvE4a3AMmC0xQ2RlFktqJcUnH4Z7wUvFpL6k1dqphTHgXkncckgEL6R%2B%2BKqYYIBDUGZRMIF2HiLwQFB6%2BqVfHkJtdF1aVmRLUEekf8ib10jqk2AsldsOW93TeLA%2ByaY17PiM%2BUTnH5OhnRXUDqrCXGQMzaV7m04TJOl0w6ZRc2Q4VG3BjN4Z5Qj02LXKR6xz6oDLSENmm7u%2Bjjcutf9NieEG1EiXMgzlQIBmjSI84FdNqrekrHsMqHiQmHQNbHYaRj1WpMe2krLKu49CwU1B3DXp%2By5nUe8nq2cMhXoi1Vr9pf%2Fhtl0UC4EZdK3bKyV4wzpHkT%2BjZu%2BEw6pA7fDLNyZvDCZoNTGBjqkAU0GKDFpf1Fhp7M4DYTeCKuI%2BT7g%2F36CSqeDHqBJPgMG10cQ5enKelcKuYqoQ%2Fi4iaiFbjADSr962Txh6M0IDv1ZH7pXXuWHOCpb8dhPmYbkq4o%2BPXpd7UWcR1a5pkbRMMq%2FDCGu%2BtJ6utZtmNajpoOt9Mg%2FNjoTadrrFWgh3j5s76XAvFMRDtrFwZ4JGUk2Hu2xd8Kb6bX5RafI7aKTQDuAZIPO&X-Amz-Signature=cb5840efe26ddc8cc928aa47ab2eb28b420b96d2ffdc32c2ad75913630e9a0ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


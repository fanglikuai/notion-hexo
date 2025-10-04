---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XPX5PEK6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEnO5LB9U83joC2%2BRZEl8qigAASakmbCZvYvrnozAN49AiBdLViB4G4iXBsXhMBPSscE3rucF3XGl6jTZED885gRqir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMRbJlw3FytIx%2BN%2BssKtwD5cjwWrAPCsJoWhmtDo%2FNux5votYCCfA5nMyWflbZtci4HfwgUPazi%2BVuzp4d%2Bzsf5BS3e49BGh61YpvbHjoretmqhjjjP%2FUyBpIl%2Fuc%2B%2FhW275vq9eA9iy6KHSHKi%2Fswh%2FCsge5OUU74lLqtfWbwOSb7RgTmUFzXq9cjajEMYHm%2B6xBdQoypGbWsdOShqMO8%2F%2BXEbh82rgydJgD3T4cus2tnNUcaMFxfU6hJ9XSMMAFl8LQQJmFjZ2y5JeMZ2%2FFa3dUFk0HCyfyJZIf0YOr2wTVn%2BrNhLU%2FhircuaVuMWSOVkf5v%2BNdtMgLjM7No5pRSSh%2FNT1hEfBsD7PCMVMbYCuAFwReOkeeeQQrqRYlhMx3mRKnVAGWua4uNNoHHLdW%2BwL3%2FUclz8Duyr5dCxn2ZX7YjAMcLXnTLf6gYP%2Fvy%2BWj1ucCDzrwThivXh1ixwM%2FEL98QhSQlFXXbGPNTVtPn6ylG1DIzmTj5hEAnzjwRd%2B5ENrBaQF%2FWABOGZM4JLb3ZDKmHvUnBCikwxhSS9RdqkKu5dHDfyEf2Tn8ToP7gjEJpQ7PdrqTLN9ZVhRbv3SCDxF88dxi22UgvbRC24sTyevdrs2fWRbETGbuj2QyMBHVSWCsoaZDf6i4oPqkwt5CFxwY6pgFUceBT8WJQar6HCM3%2BBaYLFUVC3A0UhonSdWAk4HZmDseHsd1qVeCJ8hld6CCZh5eccSzfBrluWkBKctcQUZFFi8KWeNKxSpMILwa8nC%2BUYWMIB5loiyS1RhqKBKiJPXJwEqJpozUo0zkHTFDJMqTMKcjh%2FN%2Bni4IeU9PByz2SAN6yV5GWUOKB5A9P0wJJCMMokVNR98PMNlwElwug7rZcdEf%2B2V9U&X-Amz-Signature=ff34b4ad14c7dd244eed8363deea80410a696f75b42cbeda65deac5fa6d30c33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


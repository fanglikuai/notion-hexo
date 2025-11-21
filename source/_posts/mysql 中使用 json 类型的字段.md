---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGQUK3M4%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQCqkQfxjMBaB%2B%2F4CZvp%2FvzxNHuGXajc%2BMg9oJE7Xub60gIhAKPG2H0s7bG%2FgxTUHogeT%2FzJl0RNf0aBOljaem7dtqBkKv8DCBQQABoMNjM3NDIzMTgzODA1IgxhO9UhfDvEpG1VoNsq3AP%2BLaVvfKhQtgbeNxVsvyw4CmXDriDtEygYAKAydG7yhXKO8AbEuE0xnFRUSzhgQQH2lvb6fwkTEFdvSgMiEv84YUSkqs0DYceybYtz0fAuDWN%2F9msOPA585bTbDmHcbAKN69DPdG81baMpEDQRHyBN4%2BnBaxDZ3XEr9xlwBo4wp8XURUS0Vo%2F%2BLa2HfloKaC6PioxnxMVjf79otLrpulPVun1xZj6xPb8yDX60SoMGRyEBs9Py7yy4P9ZzCxDrxIWdTJWj9WbrTviamOnR52sfpjEVCl916ZwoVxjJIG23bju%2BNacXbj5SwbVlKkNaiK2MiwN9lzTC20UL0HZMib3rwiV2wK2CzXZvJuUGbtBboGeNpIItat2jn9aO2o1cjmSwGRdlZG9DuYABGcdKbkQStrr3axxHUVrrbiXlbo6itgolSFNYR58MNO4GTUKBz6F1HmHOF1oLf7HvKOpIiIHxDllsmg7pwo7AIOmVOkaqwMAnBt6LWg3QRv2RDU2TWW6785Y8EwdRO1UwvLCB69zkzQp3JfcQNKYcBAwFvmf5RFzh1oS02b0N67WuUzQzC67mW9oLkot1ahMpjt1g3TpEGTa%2BFm9rCp2WWJpWhh%2B7%2FhQ8tnxs93EewzQbOjCh6YLJBjqkAS9FeRiQbKBS9MDpmgQk96LZx9aH44NCaJN3AxfQwJhpyGLi6Z9fr0XagVD34HA71FvltZJysrqik0I1FkCM2okvPxdG3e2EIo6%2BFsNmBTdtEEDX%2FCx5H%2BQCFl7FiFccUeAQSAvkby%2FyE8etuxjJmluTZ7TPmC4ZoW6Bd%2BMZNs110KY3Y5hZ5WmtbrqEkWb1DUHlwcZQL1QHlkY%2FGEiGW%2BR1M3Bm&X-Amz-Signature=07e8a59ac9f6a47ab92bca6ce1228ed68cb8c43e00d0b60d4df3450974aabf91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


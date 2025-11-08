---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOLJQ2L%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCID%2BuNytqZkqKjR9WDxU4tyvxy5WfYD0s5ZD3Kj4hFrlKAiEAy2zKAYe46DkIgdK3poT6cwxlBg%2Fxweoi9jy7blKN4SIqiAQI3P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOdj5PbGqoRjJtn6fSrcA0I%2BeVSxCZLGbOkzcr0ec5IClVOoWrwSpqKNw%2BLcMkLw%2B7D3TQL47qzOl8ZMGm3DT1xtjytBBEPBhoInpCIF7IXUudgyT8deChw8xYlN%2BH9anDZXsD6DCpg0Drwc2lghnUtOmMJsN3HHPq5cM4bnK9kQYmvVhcN0VREdS5VMMxDX2foz72BNaGCMhu1bpOBQ6yUvqCh2eqeLCk6R9BOer1HMSui9jOj8zRwBvALbBb10YSCgWy%2FdCfznnPNQlCYQrxAUOVDaVWYMk3%2F9jbMBLv5q%2FAO%2ByQgRaeW8gcvTYPtBB5goUKySTnHbuBf4JbNrgux%2BVKDCYZSsSUQqP6RAASxr%2BMxpD9OzpXnvy0aMoLkxt4QRGGcyYMavpIpiSCTVFVRVfMzRd6LLi53%2Fsi1BW%2FHvym%2FiGA9spILqE1evTOvWTz6wb6Osz9e5USwohfrzdV1LTNjHPwBV4U2NUwDStVPLnAsABOelZzBoxird3YIUwh5dq0y3Fld7oQCHgt5TrMJhTpVH89HrAQYnFKGn5IUBLcPKKDgC9b2NGVRN7JHE0NuCzyV5EnbhSZz6vaEfk%2F0sc4QYIw7SI4lGk%2FQmbd6xHQU%2FzMn00u7Xg9G%2Bda%2BQl4Hu6mZ7ZiKXOk5TMJGavsgGOqUBxbtGNiAkqYI9MSCBPvzm0MsJCQIziFhXxUkTfEiuH%2BpY5NE%2FYHCwMhhVKYsgo8sse%2FOmk53gMn8rfVzdP5gUI0zQcmkjWOUhUGf72lVF0kxD7S1svbXjJsys9mz682VFjgjFvYJgNbvdbaS5IjrjUBBgaDEIMd1gaAZwaN0vHpaNkbybRFGpATSo%2Bhqjlxrw2%2BoC949MYE5prcxhQjTklGFWvUKO&X-Amz-Signature=d2c0cb756e54a6d58514002d0c09ce0f69695f4fdfb52b6620b90811b743d682&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


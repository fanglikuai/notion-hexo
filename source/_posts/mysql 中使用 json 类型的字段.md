---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOVBHCGX%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T110128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIFDsgBZj8tJCRVP9QuUfLNP49Q3g3McDDVKBudMpIvArAiEAsuhWE1ZIcJE%2FL1lb8%2Fc5jEFMqIueFLSDuhgutaKTPCoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMxV%2BW5omuWcMo79kircAxKV7udXGFQVdg2ApEzRKwGu9gLSw0Wqdw%2FY%2BUThlCaCSlyeN43z5kW6oKeGgiNfYdd4nuWhqWPjLOOB8sRu095axl22e9mZJoApDIMmLqD9XBIj6%2BB3CjvAVP7d0%2B%2FFMfVbVKZivi35%2Fje0jqepJA1X14gb3LWzhe77AwaDX4misPZeCUst8Rpdvvmh9j2sP%2FjawnTMgJ0oCb3ellmDI8e%2BfOZcDcOLGkeUKQuFgupNOiTL%2BcqJDztMAQEC5hjBdxvmiB8psbO%2Be0mWlgB5fafcJJfGuCs%2FhARyVgFNcJ0aAdY%2BaQu2CH64%2BR%2FlmoKpkIg7ZmSWAdJxgPuzsuaziqyll9dbea1mKVsBAHn15tWgUW9Q7z8EfZu61i%2F3mxWS1Vomku7MbHryxAJWOFvWkneIRydUH%2F%2Bvu8J4udDlRa3U6H2bF0%2Bdd98Ken8tvf3Q0aXnDUdvxhGuW2mEfJVpoqE%2FMwTETVSKnuXaCmNXWyFML2pkAwCjs0n9A%2FWFH8yfGd6tgtQwS0rLMXQ7vumMV6HrhjSXrcSPNpQz%2F%2BsiJ%2BctsB1jBOnRfUquu4hRISbBXgEnWkULmX15la%2BxLg5CFCCLvqFdyeCM3WSNcugJ0KuyHL8LBojPQa%2Flhwa3MPey6cYGOqUBV%2BPlYvAHKPqfv9N5F9YjnVrECggvLZBv%2BWeolXXskpDgUglmG27o5lJgTHWpTBPOOXN38WcLNNkDU6kD06e%2F77MVhPPmYFtyTtUcoJPoVAiSzMSPJCTDjYDh7Kew7XZzVPa3Tsbjkh6SuFEYmMdR0c57bU1%2BwC8OZfY0dxJQo8zDKcM3wwRA8Ewu%2F6BSGXMP%2FvcHsYtwLUbdeuRHjFW%2FzJMDEG6%2B&X-Amz-Signature=4539338ca00ad63ebb9b67c826593407a9163d7fc320d8479b99984016c6138d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


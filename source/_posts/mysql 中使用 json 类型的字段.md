---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6VIRAW7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDKtpui51roPUQj0TkQ4aSwWr%2FixxaBKZbanClsvPwbsAiEAxAOPdMpWlVbgAg27ZHMPqMlJAtPPfNL5CfqaDEH4Dicq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDAopsQTTnXcVQiFjzSrcA9JyVmeSoelEYL2OUxtHTRFutnlJ%2FO4785dI3TYghYGD7iI0cJHEmIn%2BW5PUXBgFHI8DuuT12NYyyhBR7QYJpNpR3QGAbQG6LU5k1AvTevU43epO2dpaq42kfdLGe3xKT%2BoZi25CIOtFiB0vWKPk98f0abp8uunViEK%2BF65n6UnEeoXmOlP0p8mnHICbWggCQzLDAY25CTPEURs5EcUwHn%2B4bnJa9EWKHexV3r04PwamZAkWVH4gl%2BDWaxWz%2Fpzf4DOv1OOrgyLcTd1%2B81hEe6GKvvNs27om%2FccEvi9WwIJRO0D9mRL30Zr4V3BvAV4x0OigPSZKBHtMKA9ESRdrTWTDcb19ZU8WLra%2F5Z2Xd5FO4Vr7IiiqwpAFEDAtDs7uKfxGFmbsz%2BAwcgp%2FUK0CrMcHecbpULHB%2F8Nj7StLcvcuzN3zf6SZHjN6UP0Hu3FPjdbG4Jwj%2BYbgqNRRu1KK7mqYNN7ne7wDTN%2FfxfAApT9OcmKFWv%2FFsKhTfwGa%2FV86uZn1XtlqfS1Az7Z3WhaMdOXTcfcHpJlbQOCTEPMLuxgT2lVp4ZhM0hlJaxBY6%2BRJSa4XYXIMhBeQjQCPlpWZJ2BxgCXznbfaQ71gm4PHLJ8X1WOJJfK6JYjEp6pkMMj0n8gGOqUB4HiQJzve1Jljelu5aitP5K44bYp8aitc6NL90ZVxrDWKqFbpW1IH6IjGPNVT2PfEL5LTwB8f4OWJ%2FP7iQ4kntj%2BbfDHI20%2Bnt0rS4EOn58YVyXdsPtxBjJtNCwPmBRTieM9WX%2ByBhoMjqQseoxUfqjYyhYFW%2Fi%2FMwqkpYZvJoQyWUDE7tEzL3RFipWHXuOCtdM9R8RCgNRXXeh2%2Badf%2FsLtSfcW%2F&X-Amz-Signature=9ae3305cd45def13a3901e502f5739ff4023e29335acf35986cea870f11ca717&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


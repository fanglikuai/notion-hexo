---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2I56H5A%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPoRvMRoxqPDxaSYLXjjWfAXmEo5poMGiEy6Dc3LDgqAIhAIBX8FxOcRq%2BiwzD60FkZnJ7D2rcRmPIBVWISben%2BIC4Kv8DCCEQABoMNjM3NDIzMTgzODA1IgzC4TC6vBgi%2BErIODcq3AOlO18MY4cgVE3WAlHTm90TfDI3ku8LINRFlrfZ0Zeh7o1%2BX2KdEh%2B%2F48Yp5TfWUWvs6RH1yO0L3WabAmXeN%2FC5cYvkZlbgK1j6X9QY8FMZGsDhSGl8gPDLO9CplhNRj49u9uXlyT69up3%2B50kuNFYKqF%2F1hiUPzKdW1RqxtC9u78vjoBU%2FgHBGpnCUlrB4t9Fa4SjlsrhNdCITV6xYKWal4jZ7pg2GQ5oaktxVSAiRYiNkbETZy2ZWgnalBg6SurcjMBOfy3P4gnfzTN9566zcYsK76o1iAOEiJb3bnAUS6k0geu3Sa6SREQMXRH4IbcPrIQ0GSoehV340fvUGmbSTEvS57iN9DGvyt%2BdV18c6iOHkTeXEYdF%2FfaGVhUz1PPrUBv0ewJRBNw4UlVELyrJwYNb5bQdzWjF8gqlBEP1ukemK3H%2FOPnVPdoXdw1282bVTrAJ5F64H1tC%2BHquSMsk5tx3Ssv1cVnym%2BwyMDgm1Lk8%2BM3clQoM1yKGHCefViYCU4AOPCDeGXrFR6q92JvWiLCWtEmt%2B%2BCou4OZxJrjlXOCRSpdIevsp5Kq1WbhDje3QURIjspnU8t%2F8uptyJ3bMJ%2FBSKo%2FpR%2FAiY4CHjQsry8Lm2GdUNW%2F5W9O%2FkTC7%2BvbGBjqkAWPUtD4TeDtbScnPSF7nzeTMt4hOsqmfLQGBrg61XnqtbEvNUEHc0OdJlC6euJ0qGAv%2FcUc7FAVFi2UHPriTLkNXC4qi9iRi6psPpAn9ivvgW67rREoPRPwP6ggIQFBQ45yuOBO4yM5j59p1ywhfMRxbRwFd6FjDShNODdXKgNDBi4joUFPw6i9IMwv4%2BlpHvh1vjDt%2F8cnojGZ5e0wMx8Yj5wNo&X-Amz-Signature=717769fd53eff17dba956ea839eb128a3087a909231ea92f17d8228f25a5897a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


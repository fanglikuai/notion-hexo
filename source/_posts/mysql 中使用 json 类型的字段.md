---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JY73VBY%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtxX64Jp3bSSCRinITTKrj47L0zAGI%2Bh%2FmH8YR31AQ5gIgaob7m55XQklpTXIQ7mnQ2xuXNwUom1ygMbOVMfOhlgAqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDEyTsqhmJmSlLNvVyrcA0EZOrYqxCLZ2kqzJsHbF4qYhtG0MJo%2Fnc25t%2BpEm%2B8qdj7jSXrW0cQhuOcvEpWyrwSH9CXDlcBEOuu4Ld2AVkmNFab1kXAi3Wfzpk5JYL41bLVa8s%2BV7%2BSp2vppCj6ThY%2BOFrLMhZogANzNGA1De6di1OwmHzLCS%2FEX4TC03ensXozFcWQwjlAAhnG7ojpRcTFSoYwi%2BGrNHRULLYzcaH%2B%2B5Y5RECnjD0M0W2Csrc9d1JIIQpNlLnyh2afAxgHvg0SF3PE1fYIJF5nldnm52NWNA%2FyC0%2BlRQG3CFLT65IzkEp8w3mgbwzZWrKLS1ti5iL7m7iO8E%2FnTRkKM9ic0phYMzxBZ094ZO%2BxmhQe8EajeoQDzOUcJJ%2B211sPL2Sz1IepcgBYHu2BDkI8sOZH0KduD8TFOyuKixAHzDamH13d4WGg6gEmzMwamRIOhjHvrEmnP3t8FfBk90x6eBcnBQ6l4w73dicDtT%2Fnr49Y96MrYUjm7Uyo8Lq0uAZ44UG%2Be9ZIouhW9cqNE0OktxTUVrZ5aqdwSzeRKNukXA83aCnrx51Kigt5rMbwPsnhwOhrdvzpRVBhTjt2WwyGTyQU016p7ThLo8WU0IzsV79RiZlvABjhSsOcO4vZM%2BZ2MMKj%2B5cgGOqUBT2uZhBFImP6C2HfHz03hLtaHm2l2BMEY502gPTlBzhAvMBBZ5opRR%2BWRK76jrjf9GCWrHajEYsGJLeLwtsOCSbamiFnEV1LfpKNWpsI4YH1mzGvUewJWEXX07Jo6xiCMeEfDnnFRhM7ToQj9QKvhX3GY9ES30zw77sZ0JmK%2B2ZdNfJK4NBVQYuclAhr5lITYlsmbyN4kzPLUFnSejpEWsy4cQ4KU&X-Amz-Signature=af2246d604640805a39abd4e30a58ec7e4e2d66d40579cfe56e99074cf8fddc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


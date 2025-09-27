---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLJ74EA%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIDdCzzBO2ib3BP2%2FfoAvZ1fXEuP55iLHG1smz67JoBdwAiAUTkafr0uicsaSTvCXI073EijOSsJ1XxJ0mFck9CPBfCqIBAie%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJU6fefmAVMjMr7nxKtwDEgrUn2w4DSvvCwMf7E1H4OqZCGB8KmwaSOSOzdfym0Wbae24CI0R%2Brjy3wSARkZsvwY2VRdfkj%2BfHkKAIFMNSeqLbUBLFn3zwFBKRHzS%2FBdskI0GeNIYtJLGO%2Bb0jG2r5wnAJB1fAci07V8uv5HBC5ddGZ5el5jlp1eKcJuDTv2qW8K%2FkS%2Bbk7QdcWMPUzHrBdeRP7f3D%2B5CMIZq81Z216%2BGhgqg8jVs%2BZvA68nAZRm5MMp8RwX%2FC9SO5qL1G%2Bj32E0the%2F5%2B6namp2prHVzPziAmQ%2F9vLggD4ioiVKS3Y7zLzk0e332lgHR3UlGV30C1Pd7oxbgVJrygrlKdPb%2BWN7N%2FVJ4rhYr4JJ2jSpu7qX%2By0dkMCyQH2EB%2BlsngGuuOhPZv4%2FVS9IoIvvYuhe%2BzgpwkYYhdvvO%2BWd6GZrzxyuNzjzmYfB0hxjhjbthgR6vctp3vhuOFBDvivSAooYIkyzKcESiJ00rHRQ2fYoVAthsh6%2B%2FUcLc4lHRE%2B0HMC%2BeLLhDpbRwkCWtCaooVKi%2BqdSdvXT2yOE0MlH6vFmtriUauF%2FouWHdYaDpdT7IqkwxnChz7yRtzQFBI%2B6HLyNsX5lM6%2BG1Isa5Isf9Rl7PcvMJJHTP%2BCMIZw6w4HcwpNzdxgY6pgFuLBWqN5wTpqaZuBIVEvkygGuwNdUtq8D3fnB5msR%2F67HX52uimOUPEUBQESy7L3Mecjf5LxZXl6GCOtbctZOEbw4l593aJVZdXU9UF2oPbA352zzjlEtkNenOVzs%2BPTN3Nv6WusWQLxJ0xZ%2Fe33ZTwHfzfpmpeWfzF2l32QtJFrmdkbOmATPw9FmOniAGNFpYxw7IWf%2FJpeWSDJRyod2%2FkXbcV1p1&X-Amz-Signature=23018058683cc46df0e7d6c0d2ad87f33c6207b5c00f980eab91305f8ca2e43f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


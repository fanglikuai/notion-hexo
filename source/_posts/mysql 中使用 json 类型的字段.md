---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFAZZ5TT%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIHfNnwsVvnLglcXBC9ZOW7Q3aRlteTTAQubWrci9i%2FN4AiEAj38M%2BQaYQRLVywLKZzxmchkQkIAXui3mv%2FSGc7tp478q%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDIDPzLXd%2BXAiXumi9ircAwSmOvYFryQKAd0vmLA47s0kbi47KlnR6glf9zf6WulD0%2BqptMliGyFNMrjDfha%2BpQr1eHTQx5Zf1oot9g1LSenYWmUcXdo54PtUMKL38rbzQq1qgnq0SCrfUanzZ5RYXUn0OoXDIbZOHX2GV8%2FxRrAOtJT0bube4QdhNQXuZafVsrN95x98SSPGqH2gPVGkz6fi0%2BYgBEs7FeA%2BmBdW3lL7fJWb1gzJcxpu8Jpbw6SpJoQl2lcKYQZf2KxJdnaFElbT2BHUl4s50oOBDMCWgs5I4xHR5BAThcMFr63%2FCafn2YGzsuY89Up4nPGQEDL2oHrMv8iLHgfdBPQSe%2FinRuUL34RDnzD36g%2BC6ZddT0YCTnOx4EXZqmK7SBoWA%2BDE6FrgeNSFOHOkVQCB5jE4Uov03ztv7081WwPDa%2FC1zEjtEu4faNuDM2b3%2BEFtpWHc3twKpoWbzu%2FjVNrHZBwvmqSYDGZRBpoyq7TKi9jNNjXcZYMXkLwB0jqETWY9s93RKuUEXe%2BSHHZrhKMtKaMAnk96%2Bfk8TrcgVMUwwBKFscXRxKi2cVH87qdOt%2BHC7IwbEJObERyHCXGjEhZ4ZMKfzfFk23g4kMYbPHWC74z0cjktorSKH5Zv07ZO83mtMMqq%2F8gGOqUBoWVdYHM3povh3j9sJ77m8%2Bt%2Bb6X6UATD2M%2B5F0UJqfmsjWHqWCuj%2FXcID4fDvIfSkR9YW49Cwbk66sKBnLAcSr296fHbRrPskBMYt2Rfbo4PqV090S6nMQZxF74EgQRVs6ECocRfTwn93Wlt0ck%2BELDqSF12fWTmSjQTuwrk1JrDpi4iXVRVAL5zx62APD5yDBrpvZD5gmhFSKDU%2FH358A06Uy1M&X-Amz-Signature=bf75b7b603b18150b3554c4bee0aa9c38312c89001aced0e6ecf185f0bbe5570&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


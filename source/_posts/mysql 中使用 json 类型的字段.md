---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHFLQDKF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJGMEQCIDWDAGgJNz2xKi1SAlMqHpjdZPAq1%2BkF6OnnXP9EuEHhAiAsNO2NxsqB0B369crqr8u%2BAmqTxlQV4ZiobXN8gW91mSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM82RPoEl9ep%2Fjvg5xKtwDBPDf53OhoDEh14vih8ufvUcue%2Bte46eReWBgh3%2FNylcXgGmSzjPiPYw1hVSZ3UMbopWwjyDedZdSABGWiKuvC0FnjR0mLs5WxxyB5kV82kCGSXvorsyOhC%2FIPoYMLL7cq%2BeKAxKjZZzKa%2B3NT%2FxBk82LhS1hCh%2FleI4GfmFv1UhCwAVgCMc2%2FOXdXRmzmRPYC06pcyVpWPhMgeFLdzmmWXKXZ7HM0%2B%2B4qbh%2FogzxVf%2FiDRcdyCAzMQYtu9XCc2OF7PBO4FsYPBU7hQjJ4bNs81A8i2ran3pMunX6L296XH5RmTY9An4Fve4mpJEN8OjSnDaPLp1jO7dE%2BlvUT9tZlOZvpGyHdEMpT4%2BnOYQdyfguuJ%2FgLpGYMZ8HCG5X4EueN11AMiqCedyIl2yERiQ9S5UlssxaIvtmpLOJURzK8ZLVDoiecWH%2FQmMQtlN1N%2BWxgvcZcCPV%2B9cbFxJQV%2BDjy6Pi1d1pzNnX1XHh2Y4Pp5fqYmCWtrY9vWXeEhgE3OldWJFqit7FtLhDWR6KG6c0X6OftLUcZhB9Tu8PxlU8VY4IiJpG7eOZnbUqQTdgNkB9LekM1%2FQa7AG0RdJOSo9Mn6de009KIGOPOX%2BNfsYi5CZluzQxd15urYR%2F3FQw%2Bp3axgY6pgEGjB6wcroe0uTXZeUg290rzeRZpiMrfyVlDZ6VN6TjSu2tlIVmrTwYJFVzEO2R1y3Fxc8OYB2%2F%2F5GKPQ0pqdfMDZog4NLljTk4n%2FAUbxm6u70FGcYNhMIRjsUoePksbto4vyOuxbzOSYSn7phOTRKr%2Ft5Meu%2Fu1b2NCqV%2F7Q7bcPl3m%2Bh7uLQH4rb1xI%2FGqYtLw%2B%2FZggEcXiSBMMq9lztHn8VnHGlm&X-Amz-Signature=9a6bc6c8855c65e7b7774e93cf474ac2a4e06e98bca1c8db02b52b6a789db063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


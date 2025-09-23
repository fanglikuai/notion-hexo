---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SS2PELF%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpCR17vbz0vdMdIL3At9kynbkpwrpVAys%2FEAyMPvMVyAiEA62LRU6B%2FZt0y1GHKEqpw7isnvW333ZLGikbmBxyt2CEq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDFQl%2BtfJIiE%2BA8A0eCrcAzobhb174tmYWNXLCppp5wihY0AvzN0NzvQeg%2BXQyVHhjjUjEE8sWpffGBaVRpRs%2FyP60WUQM0ynKIE%2BRH9c7Y4h4YeEkX3%2BMnRFh7prERX8f72vrcLYOyPZi5y%2BbccqKihiD46rVqZ%2F8jtxte4%2BrMuC%2FU8hPwP%2FJM3MIEHtaa6BMbKCIN5yw3QZ9d4RrJ8NiXb9NWPEBjYZ3BxMrlRQ8QIn9T2r9IfB4SCaX9wjfYufTMwJxYMGTLyOvjE1TZ4Q5jjqm6Tfj7l%2FTi3iO%2F4WVEHNaLYsy832XhvwjUVvIcz98JJgwSq71XbmUMBiVFO0DXos1uTmQb%2FsUpPGKGUP8kM5GHoUsE2M%2Bk58SqPnm3%2BBPgKyoezG6dmMVqYyv7FQMofM9JkGwfEbGYITdGpRq%2FVDd5ENyhr4Dyuyjkvy0gBMdAuxUIuBWCmTn%2BZl3S9uEjSP3ligvyg7FJMDvYFUiLVc%2FuhQX7yKvbkcaVTFh27e66P5mJVZPafVqSL0%2Bi%2BntyeUvDUu%2BvTAlwVEb0KJy4TeZAvmnQVSmXbhFsZdF4%2FNTPdLN4abz4CWhvNKy38Ph6ayqQpdKszyXQT3LBz52BZ07XrgXIAu3c%2BeJIYGVYSm8I%2FZse7JgcTxgN7mMOiyyMYGOqUBtuviNzB6Js5Ijbrpt5FD%2BwyzqvEDUxS3HoKS66vH%2Bg3z2iJsmeuJS17ud90JalX8lCrYnRoyMSCtreVE1LkFw5oymI8rNS9B9hSf%2BTI4NBd%2Fqknha%2FkTtyiaeZWbTlJGZLQmA8OcSDCcwxNEOT%2F9BZShNoHntfoBw5LHP%2BPaz9DE9VHRV%2B%2BFLFsDRpuad5yZmIiSTHNe8PYG1%2FS%2B72rGmNNLGVPn&X-Amz-Signature=cac4b883acb2e331d0fa25387e69a2cf9d7bdcbbb9373955ee87f230f9f4dda1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


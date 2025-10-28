---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCMVIJU%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDHkmGKYRIjN25oP6bQNkvioVBN2lUej0bIcNDOljbaLwIhANZgsiTtce%2FpuvAnAIJfy3FGAZmS1HpFOhTVGKE4DLtmKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgztL2jR798ZpLiLNgEq3AMcUsPYCWwpLJuPU0DFwCR0S%2F0MiIU35gzxnlE%2FsVAx%2BTR3ur15QnGwJ7nsEr55rmrX2LaaXcz%2Bq56%2Baf1ZQTPIztc6NOTow9NOBAuWRLBClcKh9%2Ff83N3yfTAN7Rm827H7G33CbNq77JKQob6WTBzmF9qP7RBFEj1aG1Z7w4UA10apAIHpjktNZHArX%2BqVkB48SFHpbLEDwY72Y5bC4yWk84edbKSropIeQU1Tt66g82FsY9vhdv3lKH5X5knmQ1b9d00Xbba6%2BB5iZ0xjdxZDUOBthdO1GFzQS%2BlVLFiCuHjlRDc1eTOs3d0eRARODfrIlksQGBwFa2KlbkSrJd3TrHow3%2BQo4oSOWtjoZmcm2gsaw9bAngHbTQhHC%2FGu%2Bh8OVNdRWjdrwUuwNMauYPZEuJJ%2FAAUjX4CDryOO32zYyF8Ow05dfY%2F87jDhDKkl0us2aZXfuFHoVxTVV%2BrcShOfhEZqGcjvkerWZyo3NzOKH679KnKESVuVwHUkWZB%2FJsfH8rXx8z5BAF3XMibSfh4EC%2FY13Vfj8f50XIPmlQKOhpljfqNMuRKnNgW%2BRIL8IcPGd3cR%2BKAE5lJg%2FGd7lxUnLFy7O7AoekDmF%2FdHFqhJ0qbMRj8fNul8Za6f1TC39oPIBjqkAQUUV%2B2uz1Kufms%2FPaOLyi6QqCYNRzBDhjQ47rskWd3RxvdZWrvsW3weLTnTltnD5SvJ3jzF5zRBIJcuWvYHTPk4iqGMAJxyqlHTJzxf5Ui5WxiEIJ8rNwcZ%2BDqr4v38NLWUDgW6aKU21xzYzsnHPtZAxBRCPyb5coKFuRKYB70NafLZRKw2lmeAaihmdWu5a5t49oMkaauTkZg8512tqUg2ZU78&X-Amz-Signature=587d178408c09a9eb4167d33e11f24882ef2b4a6932dd85c9cb85c69ac10ade8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


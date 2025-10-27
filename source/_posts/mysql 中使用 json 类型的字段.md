---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIQUDSJW%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRKxi75tXMheJrLS0QKbpLpdfe375%2BAWQcz2FHDl0iGQIhAOfc4vGQKpGByqR0XlaMk%2F07pIh7ZNNWLouHbO8prJKMKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwy2Gb38awfSTtkPbkq3APu6mlkkIlM27sIMh0GZ8I5tlLgGh7C5aJ86dqfCpXS06wpPpmjlj5chk1SrRxFq9CI04kOfhMkWWvl8K68jIpxqjd4or1Jpk2j7F27G23OFdp5fgD6x0AMUoFsmEbkQdY%2Fk7Ta1H7UNKLYh5z%2FVR7D12K2qLGiWs6m3EOWHKtGxnLe6IkRhydehh3Dflf4PpMeEcbLt2zTv9j2ZxoiYX%2F3Fu7yS3x5DRXQzBFsJum6RWOBB5MmrmBtN5TL%2Fgy45iIih2YUOy7zCjJA5IJErDM9T1ZN%2BR4C1wfzfhFRG1XJDGy2oA3fFSHDY%2FTATwPtN8TPyusiR1yI6Iu%2FZ1Y50EoYn46If051QcK0xHEk6kV6xLoKPwEJgwfQkmReJicuzBS2zLkgm4GT9UdHIxoolVkqPqwsc2Z1Kx79%2BOyCLN6O6pghe76dieMw%2BRD%2FxxFvVsLu5avdfKqWftVzSLe4%2F%2Bh57wZ4AAuH03LjV%2BvGZOGhtdBxDPiFHVvMQjYobTIDCCGBJu3mbDG7%2F18eAmf3GuMG3bHQwm0kcnRX0Y7J6rJAS9UphUr2803u8byNddN%2BYPYPzlTUTYv6XYWdMgwjlwDLGlEgaVRFpCVbV5G4rafFCg18uABi4gsXURKtXDCw8%2FzHBjqkAaIBMDuZDwPX74D2y5qbkLXNHId45E%2BK1%2FV7SLymiZBB3hFlO2es8FGyLyiEAAoXfdGZe1CjDQ%2FKUnr0qHtdAMdW3FH2%2BhHxtWNH8VS25R%2BFD4jOnClG6VkUSyMusHJLXHGPFA3P%2B8SJljhlVNv00Vs%2FgD%2F0DZrYpp78PA7o%2FoTSXLdTWEvwgzWGZPaHXwCccQdFVQ4YXddbbF9t%2BGpQ8zM%2BbX%2Bv&X-Amz-Signature=64c20411c015430afd10d68d51b1b1983fdb02aa13ab5d423340aef084d390f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


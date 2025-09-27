---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQXKU3J2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDva5bAS7mEfg90nHdR47SJIhYzkoSl%2F1Q0eTwg%2FyMFMQIhAIDlrEV602NWNk9NT31IkXpLcP2UxAVHV70h1X%2BYQyiaKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw3I3QPehISJsns%2Bh0q3AMVzb0n0NPO6ldTe4s71Hlwdz3fF8CuU2cddbGfnMoVA6M%2BrXqN0Rl%2BzTnBWmsmGgPeaSChFmvQCD7zwTWsjgTUTFJLAGLSJKWKLSyOvFN1y%2F%2FqtWScHVK7dxBDaCUK0HhgLWClQUvJ1Qqfs0sWmKfTznl0vcLJfYV%2BEWQi871EIWuaZdgzxUu%2BYtYuRES07MPz7LefkFX1lSkDfOhQ3yBwr1P8BY4dksLXRnv409M5ke1H6869Nn2gWAlMEBRCFXt4A0hwqKfbc%2F4ECi4iRvVpfXUjLMMRRqt31AwVlhGAX1G9WUmXzZIZzpn3ySMoXR2BZBMYifcUC0Dmx7NCLUoanmDmVJMKbJxkjS0vZmg0aprylQmwBOwA%2Fj6w%2Bv4bnjAoPN%2BarpJ6z9G%2B4YJ%2BW6gj%2BoqukY1OkFpwgqGeByRShksl5JYimXl3oHRMSlJWDhlVLCLN4Pcj0WjurAcxuHJw5ubxeVCvp18eWH51Pm0vDN%2F2Gb2TYQ0L5MCGvaweWJUI9MW%2F1YDSsP9npuAIIYI2z9i9YST023ZvCljcpMGk5KvZo1va%2FnM0fq9cl7Qx0eYI1FvLPM87cZiHT5Il11N6qZ%2FiB3GLwilK%2FSf7XXLfrLsfaVOvPxxEtlwjzjCyod7GBjqkAYgnwMGds3FKAIWQGKhjqOZfHEtO6UDTOktP%2B1OouVJe%2FER3uVzPb2j5b40fIrHxbU%2FBoN2NLqwVmCQX0m8HJRvJIfd1Qm405ooT45P%2BThdzu9ME3Xv0HR8hjpp2qUAM%2B6dARtSggPzo%2BkaH6bhKsH%2BPBvVVqSvQMKZJTs7qTvygQml5qY5jzCL6vISgDNBP6cWbc99%2B%2BzXl8NvD7mkwVVm5C6NA&X-Amz-Signature=7a2bc7fa8689d99078213f4af16542fb9c58fb186a4948ba74a2856d53849f4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


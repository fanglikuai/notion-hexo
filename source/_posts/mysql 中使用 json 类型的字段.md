---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GLPEE7B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T010103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhawnVveyr5wQpRaoWw2k7Oyo7VRLRbk2qNBQ6uINw7AiEA9QyZz%2FagNvAIlH9MgK1Fy%2FeQ%2FLCwsbenVjMUQCpC7lwq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDKuY7%2FKUahs1puzu7CrcAxyP6C8ol0%2F%2BH6gq%2ButPyOPuILcqZkrKoHFTVrhmBl%2BQ%2BVd627Y9vzjFlCIzFSiUNpI0O3S8%2FVPavtMLjHRo%2BVodEVRjPY0zp5QUWycI4dUn5RysbSKUVIYkIcRl9xrhc6r1zo9gxIvZaQSAxUK29nRLdAaGaBemRoXYNG0k1eZsMZGgcreKd2EPT%2Bnxy%2Be0ElRP%2BskhOtXEnavhiwr6PSvnRZ1AtH57SpgUZik2HERUXJENmdEyPwJR1barTpZkusX03UekpszVZq8W7eG7Bg9Tt7zIGNihp0X2XFAndoHbxNx%2FLZrH1PzLARCTGaQX%2Bh8anr7ROkC90rT1usooiRgwXTsr9soL6GNb%2Fgy2j7AYuOCpGFPsjeol9EY4D23DrqjO5km3o7yudBGdgpHD1GWrp8vWTooLfFp2W0rPcLDKvvdGO0bn2gyoIN19hJokpl0lGm%2BgHx%2B87n4bQ04SZFoW%2FcsUXa1RCq7eEMqkW1pVraHL54i8uO6sFGLLbZiU1I2VOQhyMUfOwC2nJqpSNxu8HPRLVwjzJKAp5o%2F7EXlgSMVoDnaLG0Yb8tJtJP%2BfdfZ9LCJlEWsuUKNYMRHXK5xT7unVSYlNkzlHdrwvo0sBwWGEHFO16SBveC59MJK3tscGOqUBrRSZSgmyuuXcZ7T8g4wYHJVqwD2p99cIIugX8DtA3ysM49EcNmQ1wTvZTL3NyjAenFTvwZ1aRSuI4bvYP1Mgvemk2u2BROtsNyuTYUSsU9J7%2Fln%2BE%2F%2FTBiDIu61dtOvOiEdZ5xA0ptMOJo6kjJArISGiIhqeNTpngwgpG7soMOPTUbYSMCylsxL3YiXpYjSQkr5PbiOYVPgpm3UbtBCQ3WrXIUjS&X-Amz-Signature=5149d65e5f81c81d0d3cad69259c9e476dce50368762d3366049c40ba38c3d1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VD3FWWOH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGrb%2B5gEtNQsU2TYI7JTsXDq6wLe43QAvE%2Fi1qwIoaq2AiEAluoHwtknOcCgCwuH7o4RqEPqll5nVvgtgAOmnJV0EuMq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLMQS1U3%2BHBWPbh6vyrcAyeOYVjVlCnGiVndHrPcE84TYtQtwHzdqCDs4XrC3tnUbW8advh4WAtNUbKEp4b09zn1qeq1m3vcAjwhE8d1pIrRxz1UHVRWDLLAG0yuWNatlEn5GY8NrdywZsLNYnZRmdNxotYrScNnJhtG5nUb%2FSTC7aT%2BiEDiZNoYzf5IEsYHr1NyytMlMjwUknxC0%2FpnKxNyGXE7CWV7pmeNyLRjVvU2hEZJz6TIpU8Jo7UlhTm789HBnEGvZWVZJOID6dy0tqveyPW%2FmIbjhTRNRx%2Fx2vb5TSVQ4IJy6Ft6k4mdVDkfPh921J%2FmT5dx10Ei9y8ozE3Fdlysuu6Sh%2Bjt8TAuE5jF9JgcnECtYujPrSlGAl9IjfYxpC54SM8xc7Ju6jon58M4%2BsOd2Ry%2BOvxFBk0MKYEBSu3HK8nhXaFTFJqQYb1UhO9u3jHMmviyuaAuoAQt7N41R8guZhQ44v313lpf%2FC1iTxijOl6as4%2BZCwxm6ID43UftLIFomLsOgAnqxGV5jlxjzyO%2BjvbWFlFZCJ5z5kA7SkO34kEnSW%2BnyT91EURsUVNCIDjvOFSi4CnPmpLFmG48c0AOouow4xkHeWzw1tlOuDBlgDVB62ZSVM58jFcJpbbEKseI0Q7XaoAnMLvy3sgGOqUBLeQBFPRGQ4cZFZP3T%2BrDJx6iCYiHA%2FigJDwORsTc2iIkoPzTT1h3iQoaoCdX82nxQ5z6l%2F78d5fvpAt2I%2Bc2n%2FFCA3yldKF2JaO16iYzWCBrickkQYAFVitqIfzAmnhfdua2m51jqGD5TdPUd5k1zISOJ4XC16ILQTaA0vmoVm5aBB9NO7pFXno6btPbo6ZRJKP8F%2Bfk%2BKKXb%2FC1SjKOoEtbE%2FaA&X-Amz-Signature=b9d85d26c371dc931c54dd125c2d824a98d7da23907d3e3eb2f73c4d1db395fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


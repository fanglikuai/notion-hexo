---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUC4Q2G%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdAwdk0ur3TAc5S3uatFCVWbeXRijCCb8Ze5fOkzJXGAiBxC798hBCDuScChU%2BudC6IYhQvxbfEG5NrSwybvGTx1Cr%2FAwhzEAAaDDYzNzQyMzE4MzgwNSIMs1RdGeblQ6gll1xjKtwDCqFF8aSD1t7Yy3G4i8G4RQoBA4IiHPdYBHBSwn49Vl2bNoBMGr9mXHYAn0VnziF0BhtEuCXrZyT8A4J3f6if5T394iMkMDLAPN17FsZ4qBDirb%2Bqn%2BbN1NGszkf3CObx8PVXI9SRoyqc1Qdrye1oWpYwrgrAm7DjvntrvdC6Bnp%2Btjme8pYa87IqbbkW2p0aMDYZJ%2FzjNLeh4fRKXi4ogxeZoQ2bDh4p4w66fGdtkrrhtGb4W%2FF1GPQxwgOATCk3I4BwNtbFKo77fQ0iWGDr%2BfwZCOobRUCdIJPLrhjamv0NmiAeqsQ3JlcSLRLO1M1g3mvAvix%2FeBxNFMx4NeRhJI6ff%2Bb7LcG9Xetk7XuVMYQ74AF6menHOluB6dIy1tsaofZsuVbMszD2Q932vyXhj4GqmFLcEfO2TbriLUJcquNsk2NoiLTu0RVZPc7QnMx%2FfZW4qRno6LoGntezSVs5dcvXSd%2FXUNZr43Eh2eyjOvwiIpc8VOPBhCICp4vyM5SmAbFdmW0%2FKXH8Oo%2BC%2Fli%2BlQFNcfoj8lhWqkiDZTVKVH5pZ0v%2F4A9nk2DeUOpUAnekVFftzwuWYYpwdr8%2FOg9O1mL%2Fx6YzXP%2FDQdQZKO3mKB202CfWSyHiu4ahbAsw5pSnyAY6pgEnEcOydm8fYrF9xomqXCh3z7XRtLFyLqE%2FjbgZ%2FTbWXuHI4KgJb0itl5fcPFu6MnLo8AqkkMP9uT6r2zFOoCi3B6yX30vVEeyJmkc8alHJTc4imWPPG8n8ZaaNAI6k5djiPxCQb3EOJ91XejsWpK4N%2BxqUwGGMcXWLJRzO2vbIIhIrPSnZopv7%2Be%2FwlG5mQ930QKhH5lLAeY1sYTdZsvW0saVPi4Ua&X-Amz-Signature=2f860e4550e5efc46ac0690b1712720a67be0cf95f0740ed23e6bdb4b3bee015&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


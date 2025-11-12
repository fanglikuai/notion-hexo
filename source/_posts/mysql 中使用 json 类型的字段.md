---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBRTYT5B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T170140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCbcwMomXWuzgLHtG%2FdYPnWzKWKWiP%2BDrwCPB34fwYhEQIgJq6jBRyYNfB9e0qMM2N%2B3DM4TjS91cbgYrEkhZKtIJkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDKB1g4%2B0LSl2DALojCrcA%2BLAoLNBMmfyGYO7NLurSH4PMmcSvD1%2BcRJmdu%2FtIcALUcScSBdOlyN3qYgv1o%2Bd24otZiAVD9TB7tpvTV%2BQiaZORHXWKTOBp89DCVhIRGF0hTBpVIf15Rp3UbIQaKiPyue9nWTGa5jadF33Nvz%2FxwYWFqoA87lRGRYfVegDADEaJ3PhKexx9JYeiGu5b%2BRxcwdS%2Fi%2FO%2FZEHfcoaJ2A1QPcjtdpIrl2AMyR7Em2egNKsZZGbtUEbiaxmEJkCXKdhmCvIyvqIBPOiUoFztwOo7S7BMJUPk8SI6xTU16qeD0v2MQomwjCiDHbj3aEry%2FkGZ%2BTUWdIFPg2axpbKPC0V%2BeWEloiJ0GhU2q%2F69BtBJgm%2FJPYHw%2B1HtDe2UdcpnqaD032Swt5ldKkykryAbOuMHNnXrua37vWtuAq3AgI9Nu1kEFGRK6YywqokF13E0K%2BDD0k4kwnmKSWEPKUzIXuA9P9ljtw01FUNNwlWmyz5xHPH2E05VoQDcQA0JV804%2F6LfkS%2F6AwmU3k%2FamAS%2BsqBLqkmFNBRPeosICKst6AZ%2BLYP6gxdR0Mytoukza4n6Uozc3dy3cYCz%2Bam0o3ckBOvdPnkbMuSgh0xp6i39ttczIPVoegXYEFsYCaTFrtCMID60sgGOqUBIH8ojRFhCHbQpYKdJXv35tUAzf0cOPVZ1gT8StCWg4apZlVqZdk55fFPSwmlS9zNoux7i7N%2BpKJvxsqhZ2xolrJiRgKmIIul%2Bk2cmpWsCRzoI6Fq1vEMo8ETSDpIeZ1W1hPOmZtdAF7ajPT%2FKQ9gvH3UUc4pxyeX5wy5%2Bcy6z%2BL1wzA3myyaAINBQ5lGnlY5TOlQkD3x%2F4mzCc%2BLeKxmguzyu2Vh&X-Amz-Signature=f53bf1210e42d55a9ac5aeaab7a0472a3f166e067a865eacf4977864b57559b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


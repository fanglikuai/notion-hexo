---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHICQVM2%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmCNT%2BFAzgb3myWud9zM6NS7WsAiDDMQxUcC%2BgjEHxFwIgeE0ZD1HshT%2BHjH2Lk9z%2B4%2FLThm%2BhWZ8eq9iwMMWjyhMq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDA1T99urtOuNc%2BObXCrcA1PR3LPL2fZhG6qeV02OLzYegcUKspver%2Bo%2BMrwcNCBlWq9q6rCXjOjFwssEMVn1nGUILNl6lgjlVlCfCfTwByRfDcnNPImXEyDgc5pTZ%2FMBxNM1WfuD49eNL65sOs%2BiSl75OFS4xxt8v5WW4Saog3%2F60IJh7Z%2BLRVlS8co3hECdmlUYLhqCA%2BmOQAcQ1UL%2BqPwP%2BNRyEJJWR9gKlObg0ekg27XXtflZ0uPyITdeQkZ4bvo4tEuM7pPOf%2Bdp9yJpoSdF%2B7BdkFVBj%2F6wT8jp95rtxgMrfi4uV6ncqOG9jqXZm7exQbPbO3vt862oqvbYiRfDGAGo6IpuWyby1uZTovtEeud%2BvJOHBsQ8jB%2FyRzTkhU225zPMkFdZ%2FozE1OPAXRe9Wf2R1oTHb5NwSkh2wrHmiLjZEpI2Y4xd2cZVmmnclhJpYdQnxElSAA2SUECpYfmPVLVAVNdJ%2BSg75M4RvnJlKD%2Bb9fEtRDmE9%2BMJOdFX6ZH0ryyPKj350AWwd50%2FVH0Ay%2F%2FecXAWL92b7EYX8rLDHK8pJAIn8yJ037d2YrSM088lmfU%2FX%2B63wP0LaFT37L2qZIf6iPScNr8fsAiPJgXC619Zo93a4E18IgpDtPDLs3eXDe5Jm6soHmgkMNS0pMgGOqUB3qBFv5MtYAo6b9cm42NDpdOAocczU7aUe0CX9pJkBABlx0JYli7%2BnSNTmarIf2RD40r5jXwl3fHgPPHDcvLBcKPu%2BwSPmuXIXO2%2BMh%2FJtgipTbr5bkSBva%2FS4CH9a4lCoiENinoZs4W5%2B2pLatSWzbTsGX6e22o2MoRJCS%2Fx2u9ijKRtOrwMeD9hcX34WMAGaWO9D4Zm5x3%2BapG42%2FUaX0B3H5z4&X-Amz-Signature=a2db752bae10d70a53ff4929ebcf885c37a6377baf4b21ab417166bc929a6f89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


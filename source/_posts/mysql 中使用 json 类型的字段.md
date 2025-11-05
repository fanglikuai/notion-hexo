---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RCMD7MC%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDDloZFf5EINnO4u2xE96X4oCwmFnctbm9DsaFxvzzLvAiEAzobdK3vQAdEP3tT8A1c4pnw%2FqpOCu3Q%2FNCrlrjm8JiQqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGXnFeK3FGxgXHkclCrcA5zHV6sOAu3Kr6l2%2Bgm%2FxzubasItifrx%2Bwb5dgxMliFPV5lgADlMPSAas28z2ws%2FjtqGDdGZcbDA5%2B%2BgyQx9pNLKiBa%2FnPCmA0mT3uqk2bARhyZkiLnRdP4tktJj4NwwDhVixM6f2%2F8CTu3CyVxMF9Bq%2F7c6L875VkyeSc8G8FZ3hz6arYsnNv2VA9xhw9KhYUbqOF8nbubXjmY7mx9qEs40XH5rOI3SUR%2FW1%2BJH3loxkiNWsT1%2BOTTQWlVIPm7HgWoDPnT%2Fwd3yj5s6kFv0q0REryWb3RfAwpbqhAcwOkerp6sN9PeFPZsWxfSfWE8hZnnTGKIl3RWFiOKPVWVcQSc2dVyovp21Tq2YoCjsSQFOYbrCt2%2BeKWtltPSSAwPywpCAT4y6M3oWk8Smf4VEq2eUVTZVDg5pfJumQQASnk6A0O6DBcDo1FGrH%2Bm2jeeGgLv8kuLlCEd8ypt39SUvHt9WWtIvyd8qPigdyW0Q82giD5wybmWBg6rFGgRBHjzY2MhNeIqF5MQ3BlsAQqqpHEKz7bDzUOAnosjR2PtdihIOG%2Bqux9hSWghYZaZiPrl4XJk%2ByJ4T4OGINGjPH0HtoTxD2rBT%2BrZNZmpMegRInTYHY8oz2pZyJzB9GIbaMMXAq8gGOqUByyVx3xiK7BNLffDM741mkoquPhYKCp0kwBJzqFpYSywLQR5DPWzUmqKFJfXXgLOdtnokXT60viLGa199Q%2FoO%2BI1FCp6Hg4cODlFMBRDkWemct2JbAFF16Meaum8eXbwPhsftViJUAU8QIu%2BKJY8vBV2aSpLPaes0WRfzND7ASq60w4dL2LmO3U%2BNALBRDHvp59SM1h4nvkotgnDQGX0wWvHKto5w&X-Amz-Signature=cf199ff1f0574731606b77776da4a976aad9df5627b51b09d2ef8c77e2c00c4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


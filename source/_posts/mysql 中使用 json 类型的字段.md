---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LCFYTLE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAaW4B1q1%2FFzcSmoppxq4Pmcg0Xlp%2Fkd1VRGwcrixSNGAiEAuE89FeAZnyoJCc%2BJ0xSDGJL35PHxBym1u4fSeUWla8Aq%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDFCosLVTycVCrqeG2ircA7NOt0yyNu2X1gJtk7aCs6DdxOma9RxpE05YAJym5uHBL51rGp%2BbDz%2BXO94D8xOIikqHnrV76Tngt%2BMOp7z0sPTclA%2BL%2FF2O1XmkZ7MxHzuAXOEtxSVOHb3eYjcrIWFkeEHg%2Fd9Tm3V4uiOhM%2BInCETDrsG%2B%2FTpParqP7avNP5OSny5WZYmZaCIbMPE2V86O1Yf1XMB26eY22Qy7d0Ho%2Fba2%2FlKRsoPKf%2FgObqbX6HdHmvTHdO1lbfH6YznbQk3jnD8pr53ld7X7za8pZbnLOS%2FIIGpWHSqLAbZ0b%2BpYXWpYi6WCEVpbJ5WdJSFcWAQLlqxSR0tqEPDsCb3WyLllN3OJMdi1sbt2nxlluG5cRLa5ZPbOEs7Eug%2FZlINOZkI%2F3hpLm78Cy5f3pbxQuRx57%2F68eRoh4iwpT38zrQbzD%2BEO%2B7Q4Aw3wkZEeP%2BY2GzNWbHw%2FxH4zzg2bAYDrCejRXMnSqVgu1zXbBuR6Dy0a%2BhTBxdjOhI3qLccnX7WlV1G2unTrACURXs4eWEC9iQU2LIMfWx9Y5huv1ccUjoBu06wnX39AbnwIDwYHKL2O8X%2F7HtChD%2F60fEq0VbN26tJ6n5ffWD6x14g01%2BIAHZl7IObkcI2Yf2%2BE94iAKriDMKj%2FxsYGOqUBt699iEHh1KvEC7oSGI5iAcIF4J1NZdgih8kkaMyVAd6rBBxzs5Pb086fU6USbBa0uodkCJhGeF%2FHm3NJc65aqCTR%2BdJ0Tu2UbNXP5yyIriRggtrROqo%2F7lRFCktF4qFwWY0B%2FwCQMyer0nS0pSkRFdsdXuRCHgo205r5Xag%2FnEuMcybUV8HINFyMd7oPYI4vjUtCdLSWoS75Vj%2F6j8fZ9PzSeUs%2B&X-Amz-Signature=ff8daa4ef831f30731592b31c720cebab3a6da7de61e5d7c1decdb827e26dde7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


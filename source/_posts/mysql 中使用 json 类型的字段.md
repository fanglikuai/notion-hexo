---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGD3PEVJ%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIDZ5dyEChOiCpzWLa4t4BKNyi%2FnSMHkgBtnslwJP%2BvZJAiEA5sJ9c7y5Tgk%2Br%2FW3%2Ffksouo%2FqW32580Lpy0ntujvFBUqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEn1xJylnbuQpt9eHCrcA01sWkfH646z0G03o8XQy8UOF1gLX6Sc7p19zZhQMEV0qf%2Fkk5qzGZ%2BD47OlWGQdYL9zZep8o%2BrOCz3yO5LomCFQh2bkMbMfm%2FsRAYmCGaL1KKG%2Bl6l6UFj0NJWZJQBcvcDCqfkOWT6ksOXX710RwmCUQCvgozoG0%2Bf6j2YKgXb2JnuCxT4oIj9LsMaaJvH7gdQrMcVOM5KMUOiAQ4ABEZys3UbQLDnnihWe5mq5TWcWmakKdgm2DZPqgbB68M07sVEmq4AhvlfDjAA5Qva3XDQ%2FWBgY%2F%2F6fCt6RgkCnuueMkBQo65xCrvyAjtZVYjV7MrqrFhVoyE%2BZ2fp%2B97y0ZDmR6%2BYWZXqdpTRuB%2Bw8J%2BT5LKsDlSOPkUxHKb25RC93MKvhNkoNTNYtGXeJoECjtmYV0aJOhmaN3XynWGFYcynQQ7cFSr23%2BXdOaW2BWbE1vteBscX447aOHaqH2D6egJ6PW4DIfE4ig4fc3qJVFL7hQD%2F6kigi9o3WE9WhuiLTzl%2BuvRQ6ipl2sCee271JlCQVZOwNiFL5P9L%2BXHNmgPn0Ko711WfnOV6qlr42zqNVUaIfcrpFjJXCPmctYvr%2FvZetzpN74p38N6%2BFSc%2BevKgPw4%2FelF%2FqhWJ4UhRHMIHRnMcGOqUBEYv%2BmjJwKu%2BvHH7872cSpOPaegED08Mhjve7sY7HS%2FdDZ9K1noz%2Bk%2BsMJzm554Ww4WQEOZq%2BnYupcN1IX%2BGERHpvWyxVSNimWt3B%2Fb%2Bxj4NeF3dZI1gj0i%2BIvEM0SvByIaSexv3%2BRGi1JgXcsPlkZdVOR0XVoyXr75pYgSM15GyIbjgSyAO8mbL58uGP9y%2FPmGDs2rTWm4VFBO6hsodDLHTw717b&X-Amz-Signature=a3e76d689cc289df02c044ad221f0d65375a15cfc1021b17d4b7772259894333&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


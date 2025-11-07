---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662LZNTNL%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCec%2FzZXc9hbOPws0X%2BZqVQyId%2Bn43DBWUcImk0DnUsvAIgO7w9uIL0XE6XrATQzGwuma2FwoX4kI6I1qEIgq%2FuJScqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHmRWnvUG4l2by7H2CrcA6N2yTjmETbj2xwsnIfeEb3WyXh633fGlLbIwIA93mFnsVv%2BH%2B54R1PgksrHf6a91MnhYegaN6duLIOZ%2FqZCh3yKlwxDj%2B5eVuyxDdeAxcNndFpuQcGC5iSXCipyrCj2wDLeyFWu4AXKb95mhIonVh91QaXo0rGg7LEJC0SxB9o8abUZnP5PKMX0Q%2FNbxaV0rSdZMC%2BMnUJ0t%2BaFoViPXmsTOh0VViYtoYgM3YrlB5z6jhFu6ef%2BB29oiHyJ37PJPdZ0pEPrx8tTxrxZkFM2S%2BUKHEnDbKBDzBJxli8dWiVExEPfI5s7Te8Y8NPf2A3SYgyPCddVXSWVP69H%2FKUUsEjnMWqeFcH8W3Ms9bcbvc%2BxxWQ5LQGNMXTaQFYkMcWbsBysboOhjVb9q04Y%2BOB6ifN8m4amfMWLaR8OWp7pst9JENy%2FOBbB3TNIZ26lsnP8kqPfTM4FK%2FGAUw8uXUb1aZyNbpuGQxl90wtGjGW1s6XsHYpV4PNiWIclwGLZ0oXdO1Pf668S215RsxQOSGpqILrteOegR4eJsS%2FlmQqYCyvodCCoW4hDolIDMvmwZhMYlBj7E1mxeSgMakYidO1frxLrGQAOJdbbYVsv85JIS4V9ffY1xo8f%2FodA%2FgxQMNP6tcgGOqUBqVIzB91E4btINGY5PVCJCJqT4I66G7pFPeLQ0S14C1nwZx6lAmzn1vp%2BKNktAx3oLg%2BpsCMbbDIBSc%2FQndz68OZ8OHZ%2FMp6PYD1gNs07PDRtSiMkVaugdtKlIPQuAwxqq1tOvBwEJ2wAD1326HfURSpIXXXEaKSLL0ocUk3%2FtOCS5BwZ%2BR1Yz%2BS5Ch%2FirREor1FKPLPsEpUKlcQQ98TGGCa9o4PC&X-Amz-Signature=5736bd4b638c7077c2bf7c39a42bba87da84e18c11121395b04893099ff17fd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


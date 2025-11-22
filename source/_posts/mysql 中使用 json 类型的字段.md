---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7QPXNSQ%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCjfLtTQmEL%2FtWjENEasqR%2FKzHG5nmKpmZY3%2BAbBEP4AAIgHPCz3HwVnSw7Zhy2pK5M%2F9blES6bIQQDXqUtcs5CU4Qq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDMay%2FVbbvIrW4PtlCrcAzXYxyp68Tw6dUqdSY8h9RR7fycswP7YvmQOeT0ZMWOwJp8GvkBHczoCiVqUwzFj5%2FReSQJ%2BpCeDVUDjMkhOQACZoJ14yAyvSfdT5VnwBl4%2BCg82Z0SKYiKY6%2BuP9kBQvbyQuDrsiWo08y%2FhBmkBzRlVi%2BjXBcoXYMRkmfY6nCCdv9i%2FDWvLQA56WDGk5%2BelU4tkDNsvRhSaFRQOjvsqcXvae1VKoXnK7bENLv7W4CD5j5hZw0V2lNjmeCNIk%2FmUOoDxiO6goWUGVDp2Zyxn5IipXPghCdw%2F5KyXUzDpRl9tJTA9Pwf8%2BQ%2FwKoFIsBM2mk5R%2BkqUNZigqHyqtUwFVS5w5Ln3UywuwebmCbExhrWtvpva2wFMN3CldmeDorWBg5SfF9CCOB6kBHbO1fWBhVqLj21qVDntc5Zdnnhu2JNNT%2Flv9n94GJZnPrtcQ7XZDbFP5CeOPw5WGgYjvhUJBA6UMSMilyZHLrlYrt78y4ws3VWiVVMbBgsJeiaFnjOi6ZK3mZkft1usoXpl0RRJPq%2BVrxGQdmLX1%2BFFlEfBRjOP1FdSx0bDVUtAbKZ0XjMeYKmvxdizZtcNEpyW9e6ThLfCK3cHMWJ260%2FLnpJ%2FGKU9T4aDsWzX3nOCO0JTMLqjhckGOqUB0Bz4mD8zYkRQE%2BrJ%2FDCcJcBR7hoYoS8JX3te8q5lF6CKZ5N03GyllIaMTNH5eBdtfcXuGJu8RXuM6Dmp7Y2sk1HLVG2FPb6fhH0%2F3x087rZYp%2BebglkbR7%2FThE2KUBauJrDO53GYTtfoO6OlR0dgkoGq4lIA4ZMVsCa5ti4euT2tgoBlVZMyWRoLFVeihdWTrz33aPi%2BaoMquqEbB22C6d9UAItK&X-Amz-Signature=2519a0954b30dc7e1bdd8fa6ee0bffa8682c2df9baa2a560fe57098b6c318eec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


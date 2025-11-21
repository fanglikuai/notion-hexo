---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664V4TSD6W%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJGMEQCIAc97A4moXfQVMfclgf6KGyzh%2BtVuz9aPninsolGGb62AiBkmhu9oyZiBncVHhzmtk0gnli9siFHiVXYWPTAeVhViyr%2FAwgEEAAaDDYzNzQyMzE4MzgwNSIM6GEP5ULsQcX%2B5rFHKtwDsaEdr0Lud84HIpCSU21Yy0SNlHhheW1jGFDmswaJ4dwxx%2Ft6wygkg6wpTNJ2ZtexdyXAvTWYNFG49A1Mdj3WxG7tKpkaAVL7doTXUKMpmWRURsREoYNSwIqJ5Gh%2B0pSgzCbT3%2Bm7IdK0zAY0QCcuxHI5FvWGTyXmbkjqTtSdj60t9JKunxKvrq%2FBJ5Q6IE9XBuEpwG6eLNOaRw2uOfl9U7uc5w46d2UZ%2FHlsJcEQHQmVQfw0Bvr0aoLoXV92Z8g3kYERYFZtSfaY%2BUPPn50v1sI3nMszdtSMQQL3hfMb4n5LhIFhD6KYQ7g9Vw1BZfiUKPofl95Ndt62y1R%2BGKwaYvQUs2oe2cZ%2FlVr5rOy32dvk20i8NgUfT3gMiCQpQIatXyJyzHBWS4iHPdLFi7E4IGrApRtgtafN92uY5ej2%2FKWplguacas9QKGhaNYHvL2JYFWnVQhp36V%2Fzm6PcFrdWP91v2EK2hOm4lRbpJR2A1hyNeCRq40QcT32xPplKUaneaf45yrX9YBrrEqO1h26BNEJ8dnTZbznyQ%2FRJt4nb4V5EErPNNPRuo6%2FOPVSNgFazsXyTOKbmi7n6xChQrlPhBd04o6ZJRfQNe3kfBqyDqSMGXBJS1Lze3DLIw4ww63%2FyAY6pgG36JDeINQbfSOZj4HDRXQudz9UZPdANDTykIO5Zx9T2ZmM%2Fbuic%2BH0riBgkNY%2Fj1Idoz5Y%2FG28XeOnkh6tk073fi2gFh78Q9sIxgl76MFY7wa2psovvZYeUW5qJhqNoyWuiZF6oxe9Yun76S5fc9YuhNshBditWEK8hhDT%2F5dvXR7YR9cWTa7fu3yj9PpG1zyOefuipoEBMLhmwaZX55kFQyldy0K9&X-Amz-Signature=3239bc9785b4ffbc0a7fe8293e1240b950a196afba5e9b345d918051fa483e75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


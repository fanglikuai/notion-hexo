---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWKVEDRS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIEa1frbP9qRVmeMYYfAicxaS84W8sEPkrGumLz7NDKy8AiEAz%2FTj7xbPeaFLAeklE8QLLYLldSFp%2BTGfi33%2BYz81tjsq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDAqPbqA21fc1AL90iSrcA85kr3J4P5ncC4AH%2BBMHeQ2qRkktgKY9onmWt5O150tV7u6rpI4QhSOTw0W1YIwllaQesRB4xjB7lJGpFs988v37gthlmkT98Yrus4%2B0sCcs1WNiLyr7U27GQW9h%2Bz9%2FZjQwnntkUhRxMsNMHHeNAUEVfyD85IzsrQUGXZS3aE9vWYNhDNCGL5i8ry2bq54dY8GiFednmM1lPaAGFVDue2w9KQqYMLjC8vFlOICbtRbIvbBD5vHIDA7ZLCNyJ7y01sDBTr6%2B82CQpGN9BClQcMpEnXtzB5buWHyS8KG4U6LxE1bYiqK7%2F98H5wsphVyKgHWHog%2BOMWTaPbCyf61V72lTFvhvjVBAgF%2BYa5MoFUS5%2BE5fbZUvNGOnlDhqJlLRHaKGTGay4EOdRXMGaoAdpv9NLVuHY9DEAFrtc9miPfMp51jR8lLed2MlzwrMYzTFPOHKqsFsfFuCdGgV61Y4cD6MLs3vjWnv2kJKGJqV9Kt9PrMIBe2DwW7Bh2xhVyDScIPYwrNQ7i%2FcQb1thtt3EriorjGqHto6oXa83xhEAVyCg8B%2Fk6ic9FvXNuzNDZZ6X6WKlPKi8Ene22yPsf5YSmnPp9JIrNgkUy79FBLbIZNihtoQYkDYiInVltgqMI2c9MYGOqUBSB4EGXDWDPmZBvkMjKqoyzF4CByQr6aUz2aeqsPCI4S5MM01mQ%2BhYC2XM8VUBnBsL6hKGwdRCHy%2FczR%2BS%2Fk6ZpzHDo9pldv0y2AVOalBBkNN4QRBIdQDHLdrbG0L4ypeioJNYc%2BDMe6lNOXYbobFOe79UL2Edzi%2Bmikk2JhBSQ30VQoEKuY9jCTxMMZVJDUTkIqtKp%2BFzzT%2BntRx8msJyQcJ2ENm&X-Amz-Signature=b95fb735ac91bf66a4115de0690b7f43e04db1fb2512656d0246e2a4f98b9ebb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

